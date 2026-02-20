# FEATURE SELECTION ON HIGH-DIMENSIONAL NEURAL DATA
---

Fatma Noor, 2025

Feature selection methods for classifying finger movements from high-dimensional neural spike data.

## About the data

The dataset is provided by the International Brain Laboratory (IBL, 2023) and consists of 683 behavioural trials. Each trial is represented by 11,190 features corresponding to neuron by time-bin combinations. The task is binary classification, predicting left versus right decisions. The problem is set in a high-dimensional regime where the number of features exceeds the number of observations (p >> n).

## Results

| Method | Optimal K* | BalAcc | Runtime (h) |
|--------|-----|--------|-------------|
| Forward Stepwise Selection (FSS with AIC) | **30** | **0.774** | 5.38 |
| Gradient Boosting (GB-VI) | 35 | 0.770 | 0.15 |
| Random Forest (RF-VI) | 70 | 0.753 | 0.04 |
| Elastic Net (ENet) | 2168 | **0.805** | 1.62 |
| Lasso | 350 | 0.729 | 0.02 |
| Minimum redundancy, maximum relevance (mRMR) | 185 | 0.634 | 4.53 |

## Repo Structure
```
├── cache/                                      # Pre-computed results (.pkl files)
├── data/                                       # Dataset
├── feature_selection_report.pdf                # Full analysis report
└── high_dimensional_feature_selection.ipynb    # Main analysis notebook
```
The `cache/` directory contains stored intermediate results to avoid recomputing long-running procedures.

## Author
Fatma Noor 
London School of Economics and Political Science
