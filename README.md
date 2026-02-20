# FEATURE SELECTION ON HIGH-DIMENSIONAL NEURAL DATA

Comparative analysis of feature selection methods for classifying finger movements from neural spike count data in a high-dimensional setting.

Overview: This project evaluates six feature selection methods on a neural decoding task with extreme dimensionality (p >> n). The goal is to predict binary behavioural outcomes (left/right decisions) from mouse frontal cortex recordings.

## Dataset

- **Source**: International Brain Laboratory (IBL et al., 2023)
- **Features**: 11,190 neuron × time-bin combinations (2,238 neurons × 5 time bins, 0-500ms in 100ms windows)
- **Samples**: 683 trials
- **Target**: Binary classification (Left: 30.5%, Right: 69.5%)
- **Challenge**: Extreme p/n ratio (~16), class imbalance, distributed weak signals

## Results

| Method | Optimal K* | BalAcc | Runtime (h) |
|--------|-----|--------|-------------|
| Forward Stepwise Selection (FSS with AIC) | **30** | **0.774** | 5.38 |
| Gradient Boosting (GB-VI) | 35 | 0.770 | 0.15 |
| Random Forest (RF-VI) | 70 | 0.753 | 0.04 |
| Elastic Net (ENet) | 2168 | **0.805** | 1.62 |
| Lasso | 350 | 0.729 | 0.02 |
| Minimum redundancy, maximum relevance (mRMR) | 185 | 0.634 | 4.53 |

### Reasons underpinning these results

The dataset exhibits **distributed weak signals** with low inter-feature correlation (avg |ρ| < 0.05). This explains:
- **FSS succeeds**: Greedy AIC minimisation captures features with strongest marginal contributions
- **GB-VI succeeds**: Sequential residual fitting accumulates weak signals effectively
- **mRMR fails**: Redundancy minimisation offers no benefit when features are already independent
- **Lasso underperforms**: Hard L1 sparsity discards weakly informative features that collectively matter

## Project Structure
```
├── cache/                                      # Pre-computed results (.pkl files)
├── data/                                       # Dataset
├── feature_selection_report.pdf                # Full analysis report
└── high_dimensional_feature_selection.ipynb    # Main analysis notebook
```

⚠️ **Make sure to clone the complete repository including the `cache/` folder.** 

The cache folder contains pre-computed `.pkl` files that save **10+ hours of runtime**. Without these files, the notebook will recompute everything from scratch (FSS alone takes ~5.4 hours).

## References

- Breiman L. (2001). Random Forests. *Machine Learning*, 45(1):5-32.
- Friedman J. (2001). Greedy function approximation: A gradient boosting machine. *Annals of Statistics*, 29(5):1189-1232.
- IBL et al. (2023). A brain-wide map of neural activity during decision-making. *Nature*, 619:712-719.
- Peng H, Long F, Ding C. (2005). Feature selection based on mutual information. *IEEE TPAMI*, 27(8):1226-1238.
- Tibshirani R. (1996). Regression shrinkage and selection via the Lasso. *JRSS-B*, 58(1):267-288.
- Zou H, Hastie T. (2005). Regularization and variable selection via the Elastic Net. *JRSS-B*, 67(2):301-320.

## Author
Fatma Noor 
London School of Economics and Political Science
