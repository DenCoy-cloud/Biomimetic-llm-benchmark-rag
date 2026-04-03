# Zenodo Metadata — v4

## Upload checklist

- [ ] RAG_Does_Not_Fix_Biomimetic_v4_paper.pdf
- [ ] RAG_Does_Not_Fix_Biomimetic_v4_appendices.pdf
- [ ] Data_v4.zip (dataset_100q_pmsi.py, ground truth scripts, config)
- [ ] Résultats_v4.zip (results_rag_pmsi_20260403_041945.csv)
- [ ] biomimetic_benchmark_v4.zip (pipeline scripts)

---

## Metadata fields

**Title:**
RAG Does Not Fix What Prompting Cannot: Biomimetic Prompt Engineering on a Local PMSI Pipeline

**Authors:**
Denis Coyaud
- Affiliation: Independent Research — Architecte IA, Jedha Bootcamp / TIM senior
- ORCID: [à compléter]

**Description:**
This paper evaluates whether a local RAG pipeline on a specialized PMSI medical coding corpus changes the behavioral picture established in v2 and v3: that biomimetic prompt engineering produces no meaningful effect on local 8B-14B models. Six models from three families (Qwen3, Ministral 3, DeepSeek-R1, each in 8B and 14B variants) are evaluated across 5 configurations and 100 domain-specific questions designed by a TIM senior with 15 years of PMSI coding experience, covering four behavioral ground truth dimensions. Main result: baseline_vanilla dominates all biomimetic configurations on QUAL composite (0.375 vs 0.329-0.345) across all 6 models without exception. GT_Hallucination collapses from 0.510-0.873 (v2/v3 without RAG) to 0.140-0.247 (v4 with RAG), confirming that retrieval context does not correct hallucination behavior and amplifies it through document contamination. The 8B to 14B size increase produces no meaningful quality gain. 5 key numbers: 3,000 tests — 6 models — 5 configurations — QUAL delta vanilla vs best biomimetic = -0.030 — GT_Hallucination floor = 0.140.

**Keywords:**
- RAG
- prompt engineering
- biomimicry
- LLM behavior
- hallucination
- local models
- PMSI
- medical coding
- instruction following
- document contamination
- biomimetic prompt engineering
- local LLM
- TIM
- CIM-10

**Resource type:** Publication — Journal article / Preprint

**License:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)

**Language:** English

**Publication date:** 2026-04-[XX]

**Version:** v4

**Access:** Open

---

## Notes for upload

- Le CSV de résultats (3,000 lignes, ~3.8 Mo) peut être uploadé directement ou dans le zip Résultats_v4.zip
- Le corpus ChromaDB (58.3 Mo) : inclure la config de construction mais pas la base elle-même (trop lourde et reconstruite depuis les sources PMSI publiques)
- Vérifier la cohérence des DOIs related identifiers avec les publications v1/v2 effectives sur Zenodo

---

## Citation BibTeX

```bibtex
@article{coyaud2026rag,
  title={RAG Does Not Fix What Prompting Cannot: Biomimetic Prompt Engineering on a Local PMSI Pipeline},
  author={Coyaud, Denis},
  year={2026},
  publisher={Zenodo},
  doi={10.5281/zenodo.[DOI_V4_HERE]},
  license={CC BY-NC-SA 4.0}
}
```

---

## Narrative scientifique — contexte de la série

**v1** — Preuve de concept : l'effet biomimétique existe sur frontier (Claude Sonnet).

**v2** — Densité de prompt invariante sur 8B : enrichir le prompt ne change rien. Le plafond est dans l'instruction following.

**v3** — Le passage à 14B ne franchit pas le seuil. Sur-compression sans gain de qualité.

**v4** — RAG local sur corpus technique spécialisé : le retriever fournit du contexte, il ne corrige pas le comportement du modèle. Il le dégrade par contamination documentaire.

**v5** — Codage Python local : même familles de modèles, cas d'usage différent.

**Loi empirique complète (v1–v4) :**
Le prompt engineering biomimétique nécessite un seuil minimal d'instruction following. Négatif sous le seuil (8B, 14B, RAG). Positif au-dessus (frontier). Le seuil est localisé entre 14B et ~500B.


Versions
Version 1.0.0
10.5281/zenodo.19394811
Apr 3, 2026

Cite all versions? You can cite all versions by using the DOI 10.5281/zenodo.19394810. This DOI represents all versions, and will always resolve to the latest one. Read more.
External resources

Indexed in

    OpenAIRE

Communities

This record is not included in any communities yet.
Keywords and subjects

    RAG prompt engineering biomimicry LLM behavior local models PMSI medical coding biomimetic prompt engineering 

Details

DOI
    10.5281/zenodo.19394811
Resource type
    Software
Publisher
    Zenodo
Languages
    English 

Rights

License
    cc-by-nc-sa-4.0 icon Creative Commons Attribution Non Commercial Share Alike 4.0 International

Citation
COYAUD, D. (2026). RAG Does Not Fix What Prompting Cannot: Biomimetic Prompt Engineering on a Local PMSI Pipeline (1.0.0). Zenodo. https://doi.org/10.5281/zenodo.19394811

