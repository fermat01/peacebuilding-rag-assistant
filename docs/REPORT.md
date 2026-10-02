# RAG Evaluation Report

## Overview

This report summarizes the evaluation baseline for the
AI-Powered Peacebuilding Knowledge Assistant.

The evaluation suite measures retrieval quality, citation correctness,
groundedness, answer completeness and relevance, uncertainty handling,
multilingual consistency, robustness, and operational behavior.

The results represent the current validated baseline of the system and
are intended to detect regressions when retrieval, prompting, models,
or infrastructure change.

---

## 1. Retrieval Quality

Evaluation cases: 9 answerable questions.

| Metric | @5 | @10 | @20 |
|---|---:|---:|---:|
| Hit Rate | 0.778 | 0.889 | 1.000 |
| Recall | 0.778 | 0.889 | 1.000 |
| MRR | 0.778 | 0.790 | 0.798 |

### Interpretation

The retriever finds an expected source within the first five results
for 77.8% of the evaluation cases.

All expected sources are retrieved within the first twenty results.

---

## 2. Citation Correctness

Evaluation cases: 9.

| Metric | Result |
|---|---:|
| Citation presence | 1.000 |
| Source precision | 0.611 |
| Source recall | 0.778 |
| Valid citation rate | 1.000 |
| Fully valid responses | 1.000 |

### Interpretation

Every evaluated answer contains structurally valid citations.

Source precision remains lower than citation validity because the
retrieval context may contain additional sources beyond the expected
gold sources.

---

## 3. Groundedness

Evaluation cases: 7.

| Metric | Result |
|---|---:|
| Claim-support accuracy | 0.857 |
| Supported accuracy | 0.750 |
| Partially supported accuracy | 1.000 |
| Unsupported accuracy | 1.000 |
| Supporting-source exact match | 1.000 |

### Interpretation

The claim-support evaluation achieved 85.7% accuracy when determining
whether generated claims were supported by retrieved evidence.

---

## 4. Completeness and Relevance

Evaluation cases: 9.

| Metric | Result |
|---|---:|
| Mean concept coverage | 0.944 |
| Complete answers | 0.889 |
| Partially complete answers | 0.111 |
| Incomplete answers | 0.000 |
| Relevant answers | 1.000 |
| Irrelevant answers | 0.000 |

Regression thresholds:

- Mean concept coverage >= 0.85
- Relevant answer rate >= 0.90
- Incomplete answer rate <= 0.10
- Irrelevant answer rate <= 0.10

---

## 5. Uncertainty and Contested Questions

Evaluation cases: 2 real RAG cases.

| Metric | Result |
|---|---:|
| Appropriate uncertainty | 1.000 |
| Appropriate contestation handling | 1.000 |
| Required attribution | 1.000 |
| False certainty | 0.000 |
| Policy pass | 1.000 |

Regression thresholds:

- Appropriate uncertainty >= 0.90
- Appropriate contestation handling >= 0.90
- Required attribution >= 0.90
- False certainty <= 0.10
- Policy pass >= 0.90

---

## 6. Multilingual Evaluation

Languages: French and English.

| Metric | Result |
|---|---:|
| French expected retrieval recall | 1.000 |
| English expected retrieval recall | 1.000 |
| Expected recall gap | 0.000 |
| Source overlap | 0.750 |
| Top-source match | 1.000 |
| Concept coverage gap | 0.100 |
| Non-inconsistent answers | 1.000 |
| French language quality | 1.000 |
| English language quality | 1.000 |

Regression thresholds:

- French expected recall >= 0.90
- English expected recall >= 0.90
- Expected recall gap <= 0.10
- Source overlap >= 0.50
- Concept coverage gap <= 0.20
- Non-inconsistent answer rate >= 0.90
- French language quality >= 0.90
- English language quality >= 0.90

---

## 7. Prompt-Injection Robustness

The robustness suite contains 12 adversarial cases covering:

- user-prompt override attempts
- retrieved-document injection
- unsupported-claim pressure
- citation manipulation
- source fabrication
- system-prompt extraction

The evaluation infrastructure validates both direct attacks and
indirect attacks introduced through retrieved documents.

Deterministic propagation tests confirm that adversarial retrieved
content reaches the generation boundary without being silently altered,
allowing the generation layer to be evaluated against realistic
retrieval-side attacks.

Live external robustness-judge calibration is currently deferred.

---

## 8. Provider Consistency

The generation architecture uses a provider-independent LLM interface.

Validated implementations:

- Gemini
- OpenAI

Gemini is the currently validated production provider.

A live Gemini/OpenAI output comparison is deferred. The provider
abstraction and provider-specific usage handling are covered by the
automated test suite.

---

## 9. Operational Validation

The system tracks provider-reported token usage, including:

- input tokens
- output tokens
- total tokens
- cached input tokens where available
- reasoning tokens where available

Latency measurement uses a monotonic high-resolution performance clock.

Provider, RAG, API, persistence, and failure paths are covered by the
automated test suite.

Current automated test baseline:

**607 passing tests with Ruff clean.**

---

## 10. Known Limitations

The evaluation datasets are intentionally small and curated.

Reported percentages therefore describe the current evaluation suite
and should not be interpreted as general real-world accuracy guarantees.

Some evaluation dimensions rely on LLM-based judges. These judges are
calibrated against manually labeled examples where available but remain
model-dependent.

The multilingual benchmark currently focuses on French and English.

The robustness suite validates attack infrastructure and deterministic
propagation, while broader live-model adversarial benchmarking remains
future work.

Two corpus sources could not be automatically ingested because their
publishers returned HTTP 403 responses.

---

## Conclusion

The current baseline demonstrates measurable retrieval quality,
structured citation integrity, grounded generation, uncertainty-aware
behavior, French/English consistency, adversarial evaluation coverage,
and operational test coverage.

Future changes to retrieval, prompting, embeddings, generation models,
or infrastructure should be evaluated against these baselines and the
existing regression thresholds before release.