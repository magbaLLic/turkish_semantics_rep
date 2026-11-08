# Evaluating Turkish Semantic Representations

### A Multi-Level Empirical Evaluation of FastText, MiniLM, and BERTurk

This project investigates how different embedding architectures represent Turkish semantics across four linguistic tasks: **word-level semantic similarity, word-sense ambiguity, morphological number classification, and sentence-level semantic similarity.**

The study compares three pretrained representation models under a frozen-embedding evaluation framework:

- **FastText** – static word embeddings with character n-gram information.
- **MiniLM** – multilingual sentence-transformer embeddings.
- **BERTurk** – pretrained Turkish contextual Transformer representations.

The central finding is that no single model performs best across all tasks. Representation quality depends on the linguistic level and the objective of the evaluation.

---

## Research Questions

| ID | Research Question | Dataset / Task |
|---|---|---|
| RQ1 | How well do the models capture Turkish lexical semantic similarity? | AnlamVer |
| RQ2 | How effectively do the representations support word-sense ambiguity classification? | Turkish verb-sense dataset |
| RQ3 | To what extent is grammatical number recoverable from the representations? | UD Turkish BOUN |
| RQ4 | How well do the representations correlate with human sentence-similarity judgments? | STSb-TR |

## Models

| Model | Representation Type | Model / Configuration |
|---|---|---|
| FastText | Static, subword-based | `cc.tr.300.bin` |
| MiniLM | Multilingual sentence embeddings | `paraphrase-multilingual-MiniLM-L12-v2` |
| BERTurk | Contextual Turkish embeddings | `dbmdz/bert-base-turkish-cased` |

All representations were evaluated without end-to-end fine-tuning.

## Datasets and Evaluation

| Task | Dataset | Evaluation Metrics |
|---|---|---|
| Word Similarity | AnlamVer (500 word pairs) | Pearson correlation, Spearman correlation |
| Ambiguity Classification | 246 instances, 4 Turkish verbs | Accuracy, Macro-F1, Weighted-F1 |
| Morphological Classification | UD Turkish BOUN | Accuracy, Macro-F1, Weighted-F1 |
| Sentence Similarity | STSb-TR (1,379 test pairs) | Pearson, Spearman, MAE, RMSE |

For the classification tasks, logistic regression was trained on frozen representations. The experiments therefore measure how accessible the relevant linguistic information is to a downstream classifier.

---

## Results

### RQ1 – Word-Level Semantic Similarity

AnlamVer results:

| Model | Pearson (Clean) | Spearman (Clean) |
|---|---:|---:|
| FastText | **0.5986** | **0.5605** |
| MiniLM | 0.3628 | 0.3077 |
| BERTurk | 0.2447 | 0.3622 |

FastText achieved the strongest correlations on the clean subset, suggesting that subword information is particularly useful for isolated lexical similarity in Turkish.

### RQ2 – Word-Sense Ambiguity Classification

| Model | Accuracy | Macro-F1 | Weighted-F1 |
|---|---:|---:|---:|
| FastText | 0.2857 | 0.2373 | 0.2586 |
| MiniLM | 0.3571 | 0.3671 | 0.3447 |
| BERTurk | **0.4821** | **0.4939** | **0.4975** |

BERTurk achieved the strongest performance, consistent with the usefulness of contextual representations for distinguishing ambiguous word senses.

### RQ3 – Morphological Number Classification

| Model | Accuracy | Macro-F1 | Weighted-F1 |
|---|---:|---:|---:|
| FastText | 0.9625 | 0.9310 | 0.9636 |
| MiniLM | 0.9439 | 0.8990 | 0.9461 |
| BERTurk | **0.9779** | **0.9581** | **0.9783** |

All three models encoded information useful for grammatical-number classification. BERTurk achieved the highest overall scores.

### RQ4 – Sentence-Level Semantic Similarity

STSb-TR test results:

| Model | Spearman ρ | Pearson r | Scaled MAE | Scaled RMSE |
|---|---:|---:|---:|---:|
| FastText | 0.3791 | 0.3323 | 1.5091 | 1.8987 |
| MiniLM | **0.7726** | **0.7655** | **1.0558** | **1.3300** |
| BERTurk | 0.5494 | 0.5352 | 1.2972 | 1.6114 |

MiniLM clearly outperformed the other models on sentence-level semantic similarity.

---

## Key Findings

The experiments reveal a task-dependent performance pattern:

- **FastText** performed best on isolated word-level semantic similarity.
- **BERTurk** achieved the strongest results on ambiguity and morphological classification.
- **MiniLM** performed best on sentence-level semantic similarity.

These findings highlight the importance of evaluating embeddings at multiple linguistic levels instead of assuming that one architecture is universally optimal.

## Limitations and Future Work

Several limitations should be considered:

- The ambiguity dataset is relatively small (246 instances).
- The models use different representation extraction procedures.
- The experiments use frozen embeddings rather than end-to-end fine-tuning.
- The morphological probe uses isolated token forms.
- Confidence intervals and statistical significance tests were not conducted.

Future work could include:

- Expanding the Turkish ambiguity dataset.
- Fine-tuning models for Turkish semantic similarity.
- Training a Turkish sentence-transformer based on BERTurk.
- Conducting layer-wise morphological probing.
- Evaluating results with repeated splits and statistical uncertainty estimates.

## Technologies

- Python
- PyTorch
- Hugging Face Transformers
- Sentence-Transformers
- FastText
- Scikit-learn
- Pandas
- NumPy
- Matplotlib

## Project Context

This project was conducted as a term paper for:

**Introduction to Computational Semantics & Discourse Processing**  
M.A. Digital Humanities  
Georg-August-Universität Göttingen  
Summer Semester 2026

## Author

**Irem Yurdakul**  
M.A. Digital Humanities | Computational Linguistics & NLP

---

*This repository accompanies an empirical study of Turkish semantic representations. The reported results reflect the experimental configurations and datasets described in the accompanying term paper.*
