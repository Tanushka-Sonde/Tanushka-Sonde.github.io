---
layout: page
title: "Agentic Multimodal RAG Knowledge Assistant"
description: "Self-correcting multi-agent LangGraph RAG with hybrid text and ColPali visual retrieval."
category: Industry
importance: 1
date_range: "May – Jul 2026"
---

**May – Jul 2026** · Summer Internship, EY GDS Mumbai, Emerging Technologies in BFSI Sector Modernization

<!-- TODO: github.com/Tanushka-Sonde/Agentic-Rag returns 404. Once it is public, add `github:` to the front matter and a GitHub button here. -->

## What I did

- Architected a self-correcting **multi-agent LangGraph** pipeline unifying query rewriting, retrieval grading, and reranking under a **reflection-driven feedback loop** with **adaptive retry iterations** to boost grounding and factual accuracy.
- Engineered a **hybrid multimodal retrieval engine** fusing OpenAI **text-embedding-3-large (1536-dim)** semantic vectors with **ColPali visual embeddings (128-dim)**, plus a per-element classifier that crops and routes images, tables, and charts across **4 document formats** (PDF, PPTX, DOCX, XLSX).
- Built a production-grade **FastAPI** backend with asynchronous request cancellation, sliding-window conversational memory, and a **SHA-256**-based deduplication registry for consistent, idempotent ingestion across distributed nodes.
- Containerized the full stack — **FastAPI** backend, Streamlit UI, PostgreSQL, and Pinecone — into **5 orchestrated microservices** using **Docker Compose**, enabling scalable, fault-tolerant deployment.

**Tech:** LangGraph, OpenAI text-embedding-3-large, ColPali, FastAPI, Streamlit, PostgreSQL, Pinecone, Docker Compose
