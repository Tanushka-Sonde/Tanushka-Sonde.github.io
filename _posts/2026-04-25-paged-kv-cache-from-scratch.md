---
layout: post
title: "Paging Memory Like an OS: Building a Mini vLLM from Scratch"
date: 2026-04-25 10:00:00
description: What makes LLM serving fast, rebuilt on a tiny GPT in plain PyTorch, with every optimization measured.
tags: [llm-systems, pytorch, inference]
categories: notes
toc:
  sidebar: left
---

Training a language model and serving one are different problems. Training is one big, regular batch. Serving is a stream of requests that arrive at random times, have different prompt lengths, and stop at different points. vLLM and continuous-batching servers exist because a naive server handles that badly.

I wanted to understand those systems properly, so I rebuilt the main ideas from scratch in [tinyinfer](https://github.com/Tanushka-Sonde/Tinyinfer). It uses plain PyTorch, runs on a CPU, and serves a 1.6M-parameter GPT I trained myself. The goal was never to beat vLLM. It was to build a small, correct version of each idea and **measure** what each one actually buys.

## Three problems a naive server has

1. **Memory fragmentation.** Every request's KV cache grows by one token per step, and you don't know its final length. If you reserve the worst case up front (`prompt + max_new_tokens`), most of that reservation is never used, and it limits how many requests fit at once.
2. **Head-of-line blocking.** With static batches, a short request that lands in the same batch as a long one waits for the long one to finish.
3. **Redundant work.** Many requests start with the same system prompt, and recomputing its KV cache for every request is wasted compute.

## Idea 1: page the KV cache like an operating system

Instead of one contiguous tensor per request, the cache is a shared pool of **fixed-size blocks** of 16 tokens (vLLM's default). Each request gets a **block table** that maps its logical token positions to physical blocks, the same way a page table maps virtual addresses to page frames. Blocks are allocated only when a request actually needs one.

The trick that kept the code simple is **flat slot addressing**: a token's location is `block_id * block_size + offset`. Writing any set of tokens is then a single `index_copy_`, and reading them is a single `index_select`. My first version gathered K/V with a Python list comprehension over `(block, offset)` pairs, and the vectorized gather alone made the per-request loop noticeably faster.

The naive baseline reuses the same classes with `block_size = max_seq_len`. In other words, contiguous pre-allocation is just paging with one page per sequence, which makes the comparison fair.

On live traffic, the share of reserved KV slots that were never used dropped from **42.8% to 12.0%**.

> **A bug paging surfaced.** Cache slots were first reserved once *per layer* instead of once *per token*, so with 4 layers the position counter ran 4x too fast. Slots are now reserved once at the model level and shared by every layer. Without the exactness tests (below) I would not have caught it.

## Idea 2: decode the whole batch in one forward pass

Continuous batching only pays off if the running requests are actually computed together. `decode_batch` does one forward pass for all B running requests:

1. Reserve one slot per request and write the new K/V rows.
2. Build a padded `[B, max_len]` grid of each request's context slots, plus a validity mask.
3. Gather K/V for the whole grid with one `index_select`, then run masked attention for the batch.

Every linear layer and MLP then runs as one `[B, C]` matmul instead of B separate `[1, C]` calls.

{% include figure.liquid path="assets/img/blog/tinyinfer/throughput.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Throughput vs maximum batch size: per-request loop vs batched decode." %}

| max batch | per-request loop (tok/s) | batched decode (tok/s) | speedup |
|---:|---:|---:|---:|
| 1 | 154.0 | 161.8 | 1.05x |
| 8 | 151.9 | 447.0 | 2.94x |
| 32 | 156.9 | **665.9** | **4.24x** |

Compared with the original v1 engine (115.8 tok/s at batch 32), that is about **5.7x**.

## Idea 3: schedule one step at a time

The continuous scheduler runs a short loop every decode step:

1. **Admit** waiting requests while batch slots and KV blocks allow.
2. **Make sure** every running request can get a slot for its next token.
3. **Decode** the whole batch.
4. **Evict** finished requests and free their blocks immediately.

A request that finishes frees its blocks in the same step, and a new request can start in the next step.

The clearest demonstration is one long request (100 tokens) sharing a batch with three short ones (8 tokens):

| request | naive latency (s) | continuous latency (s) |
|---|---:|---:|
| long | 0.694 | 0.689 |
| short ×3 | 0.694 | **0.108** |

Under Poisson traffic at 12 requests/s, the median time to first token went from **0.737 s to 0.019 s**. Both engines use the same batched compute, so this difference comes purely from scheduling.

{% include figure.liquid path="assets/img/blog/tinyinfer/online.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Static vs continuous batching under live Poisson arrivals." %}

## Idea 4: cache shared prefixes by hash

When a block is full, it gets a **chained hash**: `h_i = hash(h_{i-1}, tokens_in_block_i)`. Because each hash includes the previous one, equal hashes mean an equal *entire prefix*, not just an equal block.

On admission, the scheduler walks the prompt's block hashes, adopts every match by reference (ref count + 1), and prefills only the uncached suffix. When the last owner frees a cached block, it isn't freed immediately. It sits in an LRU list and is reused only when the free list runs out, so caching never takes memory away from live requests.

With a shared 96-token system prompt, the number of prefill tokens computed fell from 6,666 to 618 (**−91%**), with a 99.7% hit rate.

The honest caveat: throughput didn't change, and the absolute time-to-first-token gain is small, because prefill on a 1.6M-parameter model is cheap. On a large model with a long system prompt, prefill dominates time to first token, and that is where this saving matters.

{% include figure.liquid path="assets/img/blog/tinyinfer/prefix.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Prefix caching off vs on, with a shared 96-token system prompt." %}

## Idea 5: admit optimistically, preempt when memory runs out

Reserving the worst case per request is safe but wasteful when requests stop early. The optimistic policy (vLLM's) admits a request as long as its prompt fits. If a later step needs a block the pool can't supply, the scheduler preempts the **newest** running request:

1. Publish its full blocks to the prefix cache.
2. Free its memory.
3. Put it back at the front of the queue.

When it is re-admitted, it re-prefills `prompt + generated_so_far`, mostly from the cache, and continues exactly where it stopped.

With a tight KV pool, this gave **+27% throughput** and a running batch **2.1x larger** from the same memory, at the cost of 22 preemptions.

{% include figure.liquid path="assets/img/blog/tinyinfer/memory.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Worst-case reservation vs optimistic admission with preemption." %}

## Proving that the optimizations change nothing

Every optimization above is only worth having if it is *exact*. The 26-test suite checks that, under greedy decoding, all of these produce **token-for-token the same output** as the reference engine:

- batched decode
- prefix caching
- preemption
- prefix caching together with preemption
- both admission policies

It also checks that paged prefill and batched decode match the dense model's logits, and that every block returns to the pool at the end. Writing these tests first made the later refactors much less scary.

## What I'd build next

- **Chunked prefill,** so a long prompt never stalls the decode batch.
- **A fused paged-attention kernel** in Triton, which reads K/V straight from blocks instead of building a padded grid.
- **Speculative decoding,** which fits naturally with a batched verifier.
- **A bigger model on a GPU,** where every one of these wins grows.

The code, benchmarks and raw results are on [GitHub](https://github.com/Tanushka-Sonde/Tinyinfer). For background, read the vLLM paper (Kwon et al., SOSP 2023) and the Orca paper (Yu et al., OSDI 2022).
