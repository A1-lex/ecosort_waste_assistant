# EcoSort: Intelligent Waste Management Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/A1-lex/ecosort_waste_assistant/blob/main/notebooks/ecosort_waste_assistant.ipynb)

An end-to-end machine learning system that classifies waste materials from images or text
descriptions and generates recycling instructions using Retrieval-Augmented Generation (RAG).

## Overview

EcoSort identifies how a waste item should be disposed of from either a photo or a written
description, then generates specific, policy-grounded disposal instructions rather than generic
advice. It combines three components behind a single interface: a CNN for image classification,
a text classifier for written descriptions, and a RAG pipeline that retrieves relevant municipal
policy documents and uses them to ground the generated instructions in real, specific detail
rather than boilerplate.

## Components

- **Image Classification**: EfficientNetB0 transfer learning (frozen head, then fine-tuned top 20 layers), 84.6-87.5% test accuracy across runs (see notebook for reproducibility discussion)
- **Text Classification**: TF-IDF + Logistic Regression, 99.9% test accuracy, compared against a MiniLM sentence-embedding baseline (98.4%)
- **Instruction Generation**: FLAN-T5 RAG system over municipal recycling policy documents, using MiniLM embeddings for retrieval and beam search decoding, tuned specifically to avoid generic boilerplate output

## Dataset

This project uses the [RealWaste dataset](https://www.mdpi.com/2078-2489/14/12/633)
(CC BY-NC-SA 4.0), not included in this repo due to size and license terms.
Download it from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/908/realwaste)
and place it in `data/RealWaste/` before running the notebook.

Citation:
> Single, S.; Iranmanesh, S.; Raad, R. RealWaste: A Novel Real-Life Data Set for Waste
> Classification Using Deep Learning. *Information* 2023, 14, 633.
> https://doi.org/10.3390/info14120633

## Setup

```bash
pip install -r requirements.txt
```

Update `DATA_ROOT` near the top of the notebook to point to wherever you've placed the
downloaded datasets.

## Results

| Component | Approach | Test Accuracy |
|---|---|---|
| Image classification | EfficientNetB0 (transfer learning + fine-tuning) | 84.6-87.5% (varies by run) |
| Text classification | TF-IDF + Logistic Regression | 99.9% |
| Text classification (baseline) | MiniLM embeddings + Logistic Regression | 98.4% |

RAG-generated instructions were evaluated for factual grounding (whether generated content
traces back to retrieved source documents) and readability (average words per sentence) across
four categories; all four passed the grounding check.

## Key Design Decisions

- **EfficientNetB0 over MobileNetV2/ResNet50**: better accuracy-to-parameter ratio for a
  relatively small dataset (~4,750 images across 9 classes), reducing overfitting risk.
- **TF-IDF over sentence embeddings for text classification**: waste descriptions are short and
  strongly lexical (category names or close synonyms often appear directly in the text), so a
  simpler, cheaper model matches or beats a semantic embedding approach here. This near-ceiling
  accuracy is also flagged as a dataset limitation, real resident-submitted text is unlikely to
  be this clean.
- **Beam search over greedy decoding for RAG generation**: greedy decoding with a low repetition
  penalty (tuned to avoid generic boilerplate output) introduced a looping artifact; beam search
  resolved this while preserving category-specific detail.
- **Checkpoint-based model selection over final-epoch weights**: both CNN training stages use
  `ModelCheckpoint` keyed to validation accuracy, since training and validation metrics diverged
  in later epochs (a mild overfitting signal documented in the notebook).

## Known Limitations

- CNN test accuracy varies meaningfully between runs (84.6-87.5%) despite a fixed random seed,
  most likely due to GPU-level nondeterminism across separate runtime sessions.
- RAG generation can occasionally surface content from an unrelated section of a correctly
  retrieved multi-topic policy document (document-level retrieval was correct, section-level
  relevance was not always enforced).
- The synthetic text dataset's near-ceiling classification accuracy likely overstates
  real-world performance; production deployment would need monitoring against actual
  resident-submitted descriptions.

## License

Code: MIT License. Dataset: see Dataset section above.
