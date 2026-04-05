# BanglaArgMine-Framework

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Data: CC BY-SA 4.0](https://img.shields.io/badge/Data-CC%20BY--SA%204.0-blue.svg)](LICENSE_DATA)

This repository is an **artifact bundle** (research/reproducibility; not a production package) for our paper:

> **Advancing Argument Mining in Bangla: A Sentence-Level Resource Framework with Dataset, Annotation Guidelines, and BanglaBERT+mBERT Baselines**

It is organised for **reproducibility**:
- the exact baseline notebooks/logs and split files used to report results

## At a glance

**BanglaArgMine** is a multi-domain Bangla argument-mining resource covering three domains (politics, health, finance and multi domains). Release v1.0 contains **38,101** instances:

- **14,836** sentences for component classification.
- **23,265** ordered sentence pairs for relation classification (balanced: Support 7,755 / Attack 7,755 / Non-related 7,755).

Baselines:
- BanglaBERT (`csebuetnlp/banglabert`) and mBERT (`google-bert/bert-base-multilingual-cased`)
- three-seed evaluation: $s \in \{42, 1337, 2025\}$
- early stopping on dev macro-F1; report test macro Precision / Recall / F1 (mean $\pm$ std)

## Tasks

- **Task 1 — Component Classification** (single sentence): `Claim` / `Premise` / `None`.
- **Task 2 — Relation Classification** (ordered sentence pair): `Support` / `Attack` / `Non-related`.
	- Pairs are **directed/ordered** and encoded as: `arg1 [SEP] arg2`.

## Results (Test Macro; Mean ± Std Over 3 Seeds)

Seeds: {42, 1337, 2025}.

| Task | Model | Precision | Recall | F1 |
|---|---|---:|---:|---:|
| Component | BanglaBERT | 0.9395 ± 0.0035 | 0.9446 ± 0.0023 | **0.9418 ± 0.0028** |
| Component | mBERT | 0.9335 ± 0.0032 | 0.9378 ± 0.0005 | 0.9355 ± 0.0017 |
| Relation | BanglaBERT | 0.9697 ± 0.0015 | 0.9695 ± 0.0016 | **0.9696 ± 0.0015** |
| Relation | mBERT | 0.9475 ± 0.0013 | 0.9470 ± 0.0015 | 0.9470 ± 0.0014 |


Harder semantic setting (Support-vs-Attack only, excluding Non-related): macro-F1 = 0.960 (BanglaBERT) and 0.930 (mBERT).

### Confusion matrices (best seed per model/task)

The paper includes a single consolidated, row-normalised 2×2 confusion figure:

![Best-seed confusion matrices (BanglaBERT + mBERT across both tasks)](paper/figures/fig03_confusion.png)

### Relation splits (included)

Relation splits (JSONL) under `supplementary/training_info/`:
- `relations_train.jsonl` (16,285)
- `relations_dev.jsonl` (3,490)
- `relations_test.jsonl` (3,490)
- Total: 23,265 (stratified 70/15/15, `random_state=42`).

Regenerate the canonical relation splits (writes outputs under `supplementary/training_info/`):

```bash
cd supplementary/training_info
python ./generate_splits.py
```

### Component data (status)

The component notebooks expect a source file (e.g., Kaggle input `arguments.json`) to generate:
`component.csv → component.jsonl → component_{train,dev,test}.jsonl`.

This public artifact bundle focuses on paper verification (notebooks/logs/figures). If you need the full component split files in-repo for review, provide the canonical component source and rerun preprocessing.

## Repository layout

- `paper/figures/` — all figures used in the manuscript.
- `supplementary/training_info/` — notebooks, logs, split files, and helper scripts used to produce and audit the baseline results.

## Citation

If you use this artifact bundle, please cite our paper (see [`CITATION.cff`](CITATION.cff)).


## Licenses & sharing notes

Licensing (summary):
- Code: MIT (see [`LICENSE`](LICENSE)).
- Data/annotations (derived artifacts): CC BY-SA 4.0 (see [`LICENSE_DATA`](LICENSE_DATA)).

The paper’s dataset card notes that annotations are intended to be released under a permissive licence (final licence confirmed at camera-ready), while the **original source text may remain under source-specific copyright**.

If you plan to make this repository public, ensure you have permission to redistribute any included text content and remove/redact files if needed.
