# Track 2: Financial Inclusion & Poverty Reduction (Rwanda)

NISR 2026 Big Data Hackathon · African Leadership University
**Team:** Marie Honette IHOZO, Peter Michael Rucakibungo

Which Rwandan households are **both poor and financially excluded**, and can a transparent, leakage-free model flag them well enough to help target financial-inclusion and social-protection programmes?

## What is in this repository
| Path | What it is |
|---|---|
| `Track2_Financial_Inclusion_Poverty.ipynb` | The full pipeline (14 documented sections), executed, with outputs |
| `logs/fit/<run>/` | TensorBoard logs from the 1-epoch placeholder run (scalars, graph, histograms, hyper-parameters, test images) |
| `outputs/` | Figures and `results_summary.json` (every number quoted in the proposal) |
| `requirements.txt` | Python packages |

## Data (not included)
EICV7 (2023/24) cross-sectional microdata from the National Institute of Statistics of Rwanda, https://statistics.gov.rw (licence CC BY 4.0). Put `Microdata.zip` next to the notebook, or the extracted `Microdata/Cross_Section/` folder, or set `NISR_DATA_DIR` to the folder that contains `CS_EICV7_poverty_file.dta`. The notebook unzips the archive automatically if needed.

## Run it
```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace Track2_Financial_Inclusion_Poverty.ipynb   # about a minute on a laptop CPU
tensorboard --logdir logs/fit                                                                  # then open http://localhost:6006
```
**Google Colab:** [Open in Colab](https://colab.research.google.com/github/IH-honnette/NISR_20226_hackathon/blob/main/Track2_Financial_Inclusion_Poverty.ipynb) (works once the repo is pushed), run all cells; Section 0 asks you to upload `Microdata.zip` (TensorFlow and TensorBoard are preinstalled). To view TensorBoard in Colab, run `%load_ext tensorboard` and `%tensorboard --logdir logs/fit`.

## Method in one paragraph
Household-level table built from EICV7 poverty, housing, person, savings, credit and programme files. Target: official poverty status (population-weighted rates reproduce NISR's published 27.4% poor and 5.4% extremely poor). Consumption, consumption quintile and programme-receipt flags are **excluded** from the inputs to prevent leakage (adding the quintile lifts logistic-regression AUC to 0.978, versus 0.789 honestly). Split by survey cluster. Models: logistic regression, random forest, and an embedding-based multi-task neural network (poor and extremely poor heads) logged to TensorBoard.

## Placeholder results (neural network trained for 1 epoch only, by design)
| Model | Test ROC-AUC | Avg. precision | Recall (poor) |
|---|---|---|---|
| Logistic regression | 0.789 | 0.538 | 0.759 |
| Random forest | 0.776 | 0.513 | 0.748 |
| Neural network (1 epoch) | 0.740 | 0.429 | 0.693 |

## Limitations
Cross-sectional survey: shows association, not causation or poverty dynamics. Programme receipt is self-reported for the last 12 months. AUC near 0.8 makes this a screening aid, not a replacement for field verification.
