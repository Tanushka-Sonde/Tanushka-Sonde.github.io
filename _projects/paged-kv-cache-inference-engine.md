---
layout: page
title: "Paged KV Cache & Continuous Batching Inference Engine"
description: "vLLM-style LLM inference engine from scratch in PyTorch: paged KV cache, continuous batching, prefix caching and preemption."
category: ML Systems
importance: 1
date_range: "Mar – Apr 2026"
github: https://github.com/Tanushka-Sonde/Tinyinfer
---

**Mar – Apr 2026** · Individual Project

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Tinyinfer"><i class="fa-brands fa-github"></i> GitHub</a> <a class="btn btn-sm z-depth-0" role="button" href="/blog/2026/paged-kv-cache-from-scratch/"><i class="fa-solid fa-pen"></i> Blog post</a>

## What I did

- Implemented a **vLLM**-style paged KV cache from scratch in raw **PyTorch** (block allocator, per-request block tables, vectorized flat-slot gather), cutting wasted KV memory from **42.8% to 12.0%**.
- Built a **continuous batching** scheduler with a **batched decode** pass for all running requests, reaching **5.7×** the throughput of the v1 engine and cutting p50 **TTFT 39×** (0.74 s → 0.019 s) under Poisson traffic.
- Added **automatic prefix caching** via chained block hashes with LRU eviction (**−91%** prefill compute) and vLLM-style **preemption** with optimistic admission (**+27%** throughput under memory pressure).
- Trained a GPT-2-style transformer with a custom byte-level **BPE tokenizer** and served it through a streaming **FastAPI** (SSE) server; **26 pytest tests** verify every optimization is token-for-token exact, with CI.

**Tech:** PyTorch, byte-level BPE tokenizer, FastAPI (SSE), pytest, GitHub Actions
