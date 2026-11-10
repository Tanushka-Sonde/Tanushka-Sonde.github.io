---
layout: page
title: "Local RAG-Based Policy Assistant"
description: "Fully local RAG chat system for HR policy queries using Ollama and ChromaDB."
category: Industry
importance: 2
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Jun – Jul 2026"
---

**Jun – Jul 2026** · Summer Internship, Deloitte Touche Tohmatsu India LLP, Technology & Transformation, Engineering

<!-- TODO: project link (MasterResume.tex has a YOUR-LINK-HERE placeholder) -->

## What I did

- Built a fully local retrieval-augmented chat system for internal HR policy queries, using **Ollama**-served embeddings and a **ChromaDB** vector store to keep sensitive documents entirely on-premise.
- Designed a document ingestion pipeline conforming to the **Open Knowledge Format (OKF)** spec, combining **PyMuPDF**, OCR fallback via **Tesseract**, and heading-aware markdown extraction across PDF, DOCX, HTML, and CSV,formats.
- Implemented an AI-assisted document splitter that falls back to LLM-proposed section boundaries when no heading structure exists, paired with a mandatory human-in-the-loop review step before any content is indexed.
- Engineered a swappable LLM backend (local **Ollama** or **OpenAI**-compatible APIs) with metadata-aware retrieval boosting by designation and region, deployed via **Docker Compose** with a **Streamlit** chat interface.

**Tech:** Ollama, ChromaDB, PyMuPDF, Tesseract, OpenAI-compatible APIs, Streamlit, Docker Compose
