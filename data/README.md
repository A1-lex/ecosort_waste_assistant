# Data

## Included in this repo
- `waste_descriptions.csv` -> 5,000 synthetically generated waste item descriptions with disposal instructions, used to train the text classification model.
- `waste_policy_documents.json` -> 14 municipal recycling policy documents used as the retrieval corpus for the RAG instruction generation system.

## Not included: RealWaste image dataset
This project uses the [RealWaste dataset](https://www.mdpi.com/2078-2489/14/12/633), licensed under CC BY-NC-SA 4.0. It is not included in this repository due to its size (~650MB) and license terms restricting redistribution.

To reproduce this project:
1. Download RealWaste from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/908/realwaste)
2. Extract it to `data/RealWaste/` following this structure:

\`\`\`
data/RealWaste/
  Cardboard/
  Food Organics/
  Glass/
  Metal/
  Miscellaneous Trash/
  Paper/
  Plastic/
  Textile Trash/
  Vegetation/
\`\`\`

### Citation
If you use the RealWaste dataset, cite the original paper:
> Single, S.; Iranmanesh, S.; Raad, R. RealWaste: A Novel Real-Life Data Set for Landfill Waste Classification Using Deep Learning. *Information* 2023, 14, 633.