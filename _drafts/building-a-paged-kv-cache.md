---
layout: post
title: "Building a paged KV cache from scratch"
tags: [ml-systems, llm-inference]
categories: [notes]
---

<!-- Outline only: write the text yourself. Points taken from the project's resume bullets. -->

- Implemented a vLLM-style paged KV cache from scratch in raw PyTorch, using a block allocator and per-request block tables to enable on-demand memory allocation with copy-on-write prefix sharing.
- Built a continuous batching scheduler that admits, decodes, and evicts requests at the granularity of individual steps, eliminating head-of-line blocking present in naive static-batch serving.
- Benchmarked both engines on synthetic Poisson-arrival traffic, measuring TTFT, TPOT, and p50/p95/p99 latency, and demonstrating substantially reduced KV cache memory waste under paging.
- Trained a GPT-2-style dense transformer baseline from scratch with a custom byte-level BPE tokenizer, then deployed the engine as a production-shaped FastAPI server with asynchronous, non-blocking request handling.
