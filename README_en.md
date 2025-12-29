# TQE: Translation-based Query Expansion

TQE (Translation-based Query Expansion) is a **translation-only query expansion module** designed for multilingual retrieval systems. It expands user queries through **faithful translation** without semantic rewriting or intent inference, minimizing semantic drift and working well in RAG and multi-stage retrieval pipelines.

---

## Motivation

In many retrieval systems, query rewrite often relies on large models for semantic paraphrasing or expansion, which can introduce several issues in practice:

- Intent drift that changes the original meaning of the query
- Mismatches with native label/index wording
- Unpredictable system behavior that is difficult to debug
- Additional model calls that increase latency and cost

Meanwhile, mainstream embedding models and large language models are trained on **multilingual corpora**, meaning semantically equivalent expressions in different languages are already aligned in the same semantic space.

Based on this fact, TQE chooses a more restrained and engineering-friendly strategy:

> **Translate faithfully only—no semantic rewrites.**

---

## Core Idea

Given a user query `q`, TQE generates a set of **semantically equivalent queries in different languages**:

- The original query (e.g., Chinese)
- Faithfully translated equivalents (e.g., English)

All queries are semantically equivalent and are used to retrieve relevant documents from different language perspectives within the same semantic space.

This approach is particularly useful for:
- Chinese users searching English corpora
- Systems whose labels or indexes are in English
- Multi-stage retrieval (label → doc → chunk)
- Vector search + rerank scenarios

---

## Features

- ✅ Performs faithful translation only; no semantic expansion
- ✅ Supports non-translatable token protection (labels, code, error messages, version numbers, etc.)
- ✅ Lightweight, pluggable translator interface
- ✅ Agnostic to specific models or frameworks
- ✅ Engineering-friendly and predictable behavior

---

## Project Structure

```text
tqe/
├── README.md
├── README_en.md
├── pyproject.toml / requirements.txt
├── tqe/
│   ├── __init__.py
│   ├── translator.py    # Translation interface with non-translatable token protection
│   ├── rewrite.py       # Translation-based Query Expansion implementation
│   ├── utils.py
│   └── demo.py          # Runnable local example
└── .gitignore
```
