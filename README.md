# biomimetic-llm-benchmark-rag-pmsi

**Biomimetic Ecological Anchoring — RAG PMSI Benchmark (v4)**

Denis Coyaud — Independent Research, Architecte IA (Jedha Bootcamp) / TIM senior

---

## Overview

This repository contains all materials for **v4** of the biomimetic prompt engineering research series. The core idea: instead of instructing a language model to "be concise," we ask it to behave as a marine organism whose natural biology embodies conciseness (e.g., a sponge that filters passively, with no brain and no verbosity).

v4 tests a direct variable: **does a local RAG pipeline on a specialized PMSI corpus change the behavioral picture established in v2 and v3?** Six models from three matched families (Qwen3, Ministral 3, DeepSeek-R1) are evaluated in both 8B and 14B variants across 5 configurations and 100 domain-specific PMSI medical coding questions.

The 100-question dataset was designed by a TIM senior with 15 years of PMSI coding experience (MCO, SSR, HAD). Ground truths validated by domain expertise. The corpus is the official PMSI reference — uncontaminated, controlled vocabulary.

---

## Papers in This Repository

### v4 — RAG Local PMSI (This release)
*RAG Does Not Fix What Prompting Cannot: Biomimetic Prompt Engineering on a Local PMSI Pipeline*

3,000 tests — 6 models (3×8B + 3×14B) — 5 configurations — 100 PMSI questions  
Main result: baseline_vanilla dominates all biomimetic configurations on QUAL composite (0.375 vs 0.329–0.345) across all 6 models without exception. GT_Hallucination collapses from 0.510–0.873 (v2/v3 without RAG) to 0.140–0.247 (v4 with RAG). RAG context does not correct hallucination behavior — it amplifies it through document contamination. The 8B→14B size increase produces no meaningful quality gain (max Δ QUAL = 0.000 on Qwen).

---

## Repository Contents

| File | Description |
|---|---|
| `RAG_Does_Not_Fix_Biomimetic_v4_paper.pdf` | Full paper — v4 |
| `RAG_Does_Not_Fix_Biomimetic_v4_appendices.pdf` | Appendices A–G — prompts, questions, results, figures, statistics |
| `Data_v4.zip` | Dataset — dataset_100q_pmsi.py, ground truth scripts, ChromaDB config |
| `Résultats_v4.zip` | CSV results — 6 models (3×8B + 3×14B), all 5 configs, 3,000 rows |
| `biomimetic_benchmark_v4.zip` | Python scripts — v4 RAG benchmark pipeline |

---

## Models Tested (v4)

| Model | Family | Size | Type | Condition |
|---|---|---|---|---|
| Qwen3 8B | Alibaba Qwen | 8B | Dense | v4 — 8B |
| Qwen3 14B | Alibaba Qwen | 14B | Dense | v4 — 14B |
| Ministral 3 8B | Mistral AI | 8B | Dense, 256k ctx | v4 — 8B |
| Ministral 3 14B | Mistral AI | 14B | Dense, 256k ctx | v4 — 14B |
| DeepSeek-R1 8B | DeepSeek | 8B | Reasoning (`<think>` filtered) | v4 — 8B |
| DeepSeek-R1 14B | DeepSeek | 14B | Reasoning (`<think>` filtered) | v4 — 14B |

All models run locally via Ollama (RTX 4060 8GB, Windows). Zero cloud dependency. Total run time: 52.9 hours.

---

## RAG Pipeline

| Component | Configuration |
|---|---|
| Vector store | ChromaDB (58.3 MB, official PMSI reference) |
| Embeddings | sentence-transformers/all-MiniLM-L6-v2 |
| Retriever | Hybrid: semantic 60% + BM25 40%, k=10 |
| Chunking | chunk_size=800, overlap=100 |
| Context length | Fixed 503 characters (min=max=503) |
| RAG coverage | 100% (3,000/3,000 responses include context) |

---

## Dataset — 100 PMSI Questions

| Block | n | Target | Trap Type |
|---|---|---|---|
| Éponge | 25 | Exact CIM-10 labels | Documentary precision |
| Poisson-pierre | 25 | Non-existent codes / false labels / impossible associations | Documentary hallucination |
| Synalpheus | 25 | False premises on PMSI coding rules | Blind submission |
| Méduse | 25 | 15 outside corpus + 10 ambiguous | Admission of ignorance / binary decision |

Dataset designed by a TIM senior with 15 years of MCO, SSR, and HAD coding experience. Ground truths validated by domain expertise. Source: `dataset_100q_pmsi.py`.

---

## Key Results — v4

### QUAL Composite — All Configurations

| Config | Tokens | Defects | EM | Dec | Hal | Cor | QUAL |
|---|---|---|---|---|---|---|---|
| **baseline_vanilla** | **87.0** | **0.02** | **0.240** | **0.240** | **0.147** | **0.873** | **0.375** |
| poisson_pierre_meduse | 85.9 | 0.01 | 0.213 | 0.193 | 0.140 | 0.833 | 0.345 |
| synalpheus_solo | 148.6 | 0.01 | 0.220 | 0.140 | 0.247 | 0.773 | 0.345 |
| symbiose_eponge_synalpheus | 93.9 | 0.01 | 0.240 | 0.180 | 0.167 | 0.767 | 0.338 |
| poisson_pierre_solo | 110.3 | 0.00 | 0.190 | 0.167 | 0.153 | 0.807 | 0.329 |

**baseline_vanilla = best QUAL for all 6 models without exception.**

### GT Hallucination — RAG vs No-RAG

| Config | v3 8B (no RAG) | v4 (RAG) | Collapse |
|---|---|---|---|
| poisson_pierre_solo | 0.873 | 0.153 | −0.720 |
| baseline_vanilla | 0.510 | 0.147 | −0.363 |
| symbiose_eponge_synalpheus | 0.633 | 0.167 | −0.466 |

### Size Effect — 8B vs 14B (RAG PMSI)

| Family | QUAL 8B | QUAL 14B | Δ | p | Verdict |
|---|---|---|---|---|---|
| Qwen3 | 0.381 | 0.381 | 0.000 | 1.000 ns | No effect |
| Ministral 3 | 0.372 | 0.371 | −0.001 | 0.951 ns | No effect |
| DeepSeek-R1 | 0.297 | 0.277 | −0.020 | 0.286 ns | No effect |

### Detector Sensitivity (PMSI Domain)

| Detector | F1 v4 RAG | F1 v2/v3 general | Status |
|---|---|---|---|
| Hallucination | 0.926 | 0.837–0.861 | **RELIABLE** |
| Blind Submission | 0.000 | 0.872–0.912 | **FAILED** |
| Equivocation | 0.000 | 0.006–0.035 | **FAILED** |

---

## Empirical Law — Updated After v4

| Condition | Biomimetic Effect | RAG Effect |
|---|---|---|
| 8B local, no RAG (v2) | Negligible | N/A |
| 14B local, no RAG (v3) | Negative — 14B inferior to 8B | N/A |
| 8B+14B local, RAG PMSI (v4) | Negative — vanilla dominates | Degrades hallucination |
| Frontier ~500B+ (v1) | Positive | Not tested |

---

## Sovereignty-Reliability Paradox

For PMSI data (nominative medical records):

| Option | Sovereignty | RAG Reliability | Cost |
|---|---|---|---|
| Local 8–14B (v4) | Total | GT_Hal: 0.140–0.247 — coin flip or worse | 0 EUR |
| Frontier cloud | None (CLOUD Act) | Unknown — untested | API cost |
| Sovereign cloud H100 (OVH) | Total | Unknown — 30B–70B untested | ~30,000 EUR/year |

**Conclusion:** The TIM remains irreplaceable for PMSI coding in 2026 — not by ideology, but by arithmetic.

---

## Technical Notes

**DeepSeek-R1 (8B and 14B):** internal `<think>...</think>` block filtered via regex before evaluation. Token count computed as word count post-filter (`len(split())`).

**num_predict cap:** `num_predict: 1024` applied uniformly across all models and configurations.

**Run time:** 52.9 hours total on RTX 4060 8GB (Windows). DeepSeek-R1 14B (reasoning model) is the primary contributor to run time.

**Detector domain-specificity:** Blind submission (F1=0.000) and equivocation (F1=0.000) detectors fail on PMSI vocabulary. GT_Correction and GT_Decision remain valid ground truth measures.

---

## Pipeline Scripts (v4 RAG)

```
5_test_paired_rag.py        — Main test runner (RAG pipeline)
6_analyze_paired.py         — Analysis script (adapted for v4 RAG columns)
7_sensitivity_analysis.py   — Detector sensitivity
8_anchor_depth_analysis_rag.py — Size effect analysis (8B vs 14B)

config_models.py            — Model configuration + Ollama calls
config_prompts_full.py      — 5 system prompt configurations
dataset_100q_pmsi.py        — 100 PMSI questions (4 blocks × 25)
ground_truth_eponge_rag.py  — GT Exact Match scoring
ground_truth_poisson_pierre_rag.py — GT Hallucination scoring
ground_truth_synalpheus_rag.py     — GT Correction scoring
ground_truth_meduse_rag.py  — GT Decision scoring
```

---

## Roadmap

| Paper | Variable | Status |
|---|---|---|
| v1 | Bidirectional validation — 9 models | Published, DOI |
| v2 | Prompt density — 6×8B | Published, DOI |
| v3 | Model size — 3×8B vs 3×14B | Published, DOI |
| v4 | RAG local PMSI | **This release** |
| v5 | Local Python coding benchmark | Planned |

---

## Citation

```
COYAUD, D. (2026). RAG Does Not Fix What Prompting Cannot: Biomimetic Prompt
Engineering on a Local PMSI Pipeline. [v4]. Zenodo.
https://doi.org/[DOI_V4_HERE]
```

---

## Related Repositories

- v2 — Prompt Density: [biomimetic-llm-benchmark-density](https://github.com/DenCoy-cloud/Biomimetic-llm-benchmark-density)
- v3 — Model Size: [biomimetic-llm-benchmark-size](https://github.com/DenCoy-cloud/Biomimetic-llm-benchmark-size)

---

## License

This work is licensed under **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**.

You are free to share and adapt the material for non-commercial purposes, provided you give appropriate credit and distribute any derivatives under the same license.

Full license: [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

---

*Pores ouverts. Pince armée. Canaux propres.*
