# EcoSort: Intelligent Waste Management Assistant
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/A1-lex/ecosort-waste-assistant/blob/main/notebooks/ecosort_waste_assistant.ipynb)

An end-to-end machine learning system that classifies waste materials from images or text
descriptions and generates recycling instructions using Retrieval-Augmented Generation (RAG).

## Overview
[2-3 sentences: what it does, the three components -> CNN, text classifier, RAG]

## Components
- **Image Classification**: EfficientNetB0 transfer learning, 87.9% test accuracy
- **Text Classification**: TF-IDF + Logistic Regression, 99.9% test accuracy
- **Instruction Generation**: FLAN-T5 RAG system over municipal recycling policy documents

## Dataset
This project uses the [RealWaste dataset](https://www.mdpi.com/2078-2489/14/12/633)
(CC BY-NC-SA 4.0), not included in this repo due to size and license terms.
Download it from [source] and place it in `data/RealWaste/` before running the notebook.

Citation:
[RealWaste paper citation from their README]

## Setup
\`\`\`bash
pip install -r requirements.txt
\`\`\`

## Results
[Brief table: CNN accuracy, text classifier accuracy, RAG evaluation summary]

## Key Design Decisions
[3-4 bullets pulled from your notebook's justifications — EfficientNetB0 choice,
TF-IDF over embeddings, FLAN-T5 with retrieval, etc.]

## License
Code: MIT License. Dataset: see Dataset section above.