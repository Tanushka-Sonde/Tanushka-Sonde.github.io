---
layout: page
title: "Paged KV Cache & Continuous Batching Inference Engine"
description: "vLLM-style paged KV cache and continuous batching scheduler built from scratch in PyTorch."
category: ML Systems
importance: 1
date_range: "Mar – Apr 2026"
github: https://github.com/Tanushka-Sonde/Tinyinfer
---

**Mar – Apr 2026** · Individual Project

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Tinyinfer"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Implemented a **vLLM**-style paged KV cache from scratch in raw **PyTorch**, using a block allocator and per-request block tables to enable on-demand memory allocation with **copy-on-write** prefix sharing.
- Built a **continuous batching** scheduler that admits, decodes, and evicts requests at the granularity of individual steps, eliminating head-of-line blocking present in naive static-batch serving.
- Benchmarked both engines on synthetic Poisson-arrival traffic, measuring **TTFT**, **TPOT**, and **p50/p95/p99** latency, and demonstrating substantially reduced KV cache memory waste under paging.
- Trained a GPT-2-style dense transformer baseline from scratch with a custom byte-level **BPE tokenizer**, then deployed the engine as a production-shaped **FastAPI** server with asynchronous, non-blocking request handling.

**Tech:** PyTorch, byte-level BPE tokenizer, FastAPI
