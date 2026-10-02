
# RAG Evaluation

## Overview

Evaluation is a first-class component of the AI-Powered Peacebuilding
Knowledge Assistant.

Rather than evaluating the system using a single end-to-end score, the
evaluation framework separates retrieval, generation, citation, multilingual,
uncertainty, and robustness behavior.

> **Important**
>
> The reported results come from small, curated evaluation datasets designed
> for engineering validation. They should not be interpreted as general
> accuracy guarantees over all possible questions.

---

## Evaluation Dimensions

The project evaluates:

1. Retrieval
2. Citation quality
3. Groundedness
4. Completeness and relevance
5. Uncertainty handling
6. Multilingual FR/EN behavior
7. Robustness
8. Provider consistency

---

## Retrieval Evaluation

Retrieval evaluation measures whether expected evidence appears among the
chunks returned by the vector search layer.

Metrics include:

- Hit@K
- Recall@K
- MRR@K

### Selected Results

| Metric | Result |
|---|---:|
| Hit@5 | 0.778 |
| Recall@5 | 0.778 |
| MRR@5 | 0.778 |
| Hit@10 | 0.889 |
| Recall@10 | 0.889 |
| MRR@10 | 0.790 |
| Hit@20 | 1.000 |
| Recall@20 | 1.000 |
| MRR@20 | 0.798 |

These measurements were calculated on a curated set of nine answerable
evaluation cases.

### Why multiple K values?

A single `K` can hide useful information.

For example, comparing `K=5`, `K=10`, and `K=20` helps determine whether
relevant evidence exists in the index but is ranked too low.

---

## Citation Evaluation

Citation evaluation checks whether generated answers contain traceable and
valid references to retrieved sources.

Selected results:

| Metric | Result |
|---|---:|
| Citation presence | 1.000 |
| Source precision | 0.611 |
| Source recall | 0.778 |
| Valid citation rate | 1.000 |
| Fully valid responses | 1.000 |

The evaluation distinguishes citation validity from citation coverage.

A citation can be structurally valid while the set of cited sources may still
have imperfect precision or recall.

---

## Groundedness

Groundedness measures whether generated claims are supported by retrieved
evidence.

Selected results from the curated groundedness evaluation:

| Metric | Result |
|---|---:|
| Claim-support accuracy | 0.857 |
| SUPPORTED classification | 0.750 |
| PARTIALLY_SUPPORTED classification | 1.000 |
| UNSUPPORTED classification | 1.000 |
| Supporting-source exact match | 1.000 |

Groundedness evaluation is important because fluent language generation alone
does not demonstrate factual support.

---

## Completeness and Relevance

This evaluation measures whether the response covers expected concepts while
remaining relevant to the question.

Selected results:

| Metric | Result |
|---|---:|
| Mean concept coverage | 0.944 |
| Complete | 0.889 |
| Partially complete | 0.111 |
| Relevant | 1.000 |

These results were measured over nine curated evaluation cases.

---

## Uncertainty Handling

Some questions should not result in confident answers.

The evaluation therefore checks whether the system appropriately communicates
uncertainty, contestation, or insufficient evidence.

Selected results:

| Metric | Result |
|---|---:|
| Appropriate uncertainty / attribution | 1.000 |
| False certainty | 0.000 |
| Policy pass rate | 1.000 |

This evaluation used a small targeted set of uncertainty cases.

---

## Multilingual Evaluation

The assistant supports both French and English.

Multilingual evaluation checks whether semantically equivalent FR/EN questions
produce comparable retrieval and answer behavior.

Selected results:

| Metric | Result |
|---|---:|
| FR expected recall | 1.000 |
| EN expected recall | 1.000 |
| Retrieval recall gap | 0.000 |
| Source overlap | 0.750 |
| Top-source match | 1.000 |
| FR concept coverage | 0.733 |
| EN concept coverage | 0.633 |
| Concept coverage gap | 0.100 |
| Non-inconsistent responses | 1.000 |

The current multilingual evaluation uses two curated FR/EN question pairs, so
these results are engineering signals rather than broad language-performance
claims.

---

## Robustness Evaluation

The robustness framework includes adversarial scenarios designed to test
whether retrieved content or user input can interfere with the intended RAG
behavior.

The evaluation set includes 12 adversarial cases covering areas such as:

- direct prompt injection
- indirect prompt injection
- malicious instructions embedded in retrieved content
- attempts to override system constraints
- propagation of untrusted instructions

The project includes deterministic robustness tests so critical control-flow
behavior can be validated without depending entirely on external LLM judges.

Live external judge calibration remains separate from deterministic regression
testing.

---

## Why Evaluation Is Split by Component

An end-to-end failure can originate from several places:

```text
Question
   ↓
Embedding
   ↓
Retrieval ──────── retrieval failure?
   ↓
Evidence
   ↓
Generation ─────── groundedness failure?
   ↓
Citations ──────── citation failure?
   ↓
Answer