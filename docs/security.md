


# Security and Robustness

## Overview

Security for an LLM application extends beyond traditional API security.

A RAG system must consider both conventional application threats and
AI-specific risks such as prompt injection, untrusted retrieved content,
unsupported generation, provider failures, and uncontrolled resource usage.

This document describes the security and robustness principles used by the
project without exposing private implementation details or credentials.

---

## Security Boundaries

The system can be viewed as several trust boundaries:

```text
User Input
    │
    ▼
FastAPI Boundary
    │
    ▼
RAG Orchestration
    │
    ├────► External LLM Provider
    │
    ▼
Retrieved Documents
    │
    ▼
PostgreSQL + pgvector