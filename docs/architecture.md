# System Architecture

## Overview

The AI-Powered Peacebuilding Knowledge Assistant is a production-oriented
multilingual Retrieval-Augmented Generation (RAG) system designed to answer
questions over a curated corpus of public peacebuilding documents related to
the Central African Republic.

The system separates document ingestion from online query processing.

- **Offline ingestion** prepares, chunks, embeds, and stores documents.
- **Online RAG** retrieves relevant evidence and generates grounded answers
  with structured citations.

The complete application source code is maintained in a private repository.
This document describes the architecture and engineering decisions of the
implemented system.

---

## High-Level Architecture

```mermaid
flowchart TB

    SOURCES["Knowledge Sources<br/>PDF · Web · Local"]

    subgraph INGEST["Offline Ingestion"]
        direction LR
        LOAD["Load & Parse"]
        CHUNK["Chunk"]
        EMBED["Embed"]

        LOAD --> CHUNK --> EMBED
    end

    DB[("PostgreSQL + pgvector<br/>Chunks · Embeddings · HNSW")]

    subgraph ONLINE["Online RAG Pipeline"]
        direction LR
        QUERY["Query<br/>Embedding"]
        RETRIEVE["Vector<br/>Retrieval"]
        GENERATE["Grounded<br/>Generation"]
        CITE["Structured<br/>Citations"]

        QUERY --> RETRIEVE --> GENERATE --> CITE
    end

    PROVIDERS["AI Providers<br/>Gemini · OpenAI"]
    APP["FastAPI + SSE<br/>Chat API"]

    EVAL["Evaluation Framework<br/>Retrieval · Groundedness · Citations<br/>FR/EN · Uncertainty · Robustness"]

    SOURCES --> INGEST
    INGEST --> DB
    DB --> ONLINE

    APP --> QUERY
    PROVIDERS --> GENERATE
    CITE --> APP

    ONLINE -.-> EVAL