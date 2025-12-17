# FEATURE SELECTION ON HIGH-DIMENSIONAL NEURAL DATA

Comparative analysis of feature selection methods for classifying finger movements from neural spike count data in a high-dimensional setting.

## Overview

This project evaluates six feature selection methods on a neural decoding task with extreme dimensionality (p >> n). The goal is to predict binary behavioural outcomes (left/right decisions) from mouse frontal cortex recordings.

## Dataset

- **Source**: International Brain Laboratory (IBL et al., 2023)
- **Features**: 11,190 neuron × time-bin combinations (2,238 neurons × 5 time bins, 0-500ms in 100ms windows)
- **Samples**: 683 trials
- **Target**: Binary classification (Left: 30.5%, Right: 69.5%)
- **Challenge**: Extreme p/n ratio (~16), class imbalance, distributed weak signals

## Feature selection methods

| Method | Type | Description |
|--------|------|-------------|
| Forward Stepwise Selection (FSS) | Wrapper | Greedy forward search minimising AIC |
| mRMR | Wrapper | Minimum redundancy, maximum relevance |
| Lasso | Embedded (shrinkage) | L1 regularisation |
| Elastic Net | Embedded (shrinkage) | L1 + L2 regularisation |
| Random Forest VI | Embedded (tree) | Variable importance from RF |
| Gradient Boosting VI | Embedded (tree) | Variable importance from GB |

## Results

| Method | K* | BalAcc | Runtime (h) |
|--------|-----|--------|-------------|
| **FSS** | **30** | **0.774** | 5.38 |
| GB-VI | 35 | 0.770 | 0.15 |
| RF-VI | 70 | 0.753 | 0.04 |
| ENet | 2168 | **0.805** | 1.62 |
| Lasso | 350 | 0.729 | 0.02 |
| mRMR | 185 | 0.634 | 4.53 |

### Findings

- **Best overall trade-off**: FSS (K*=30, BalAcc=0.774) - highest composite score balancing sparsity, accuracy, and runtime
- **Best for generalisation**: GB-VI (K*=35) - highest average BalAcc across diverse classifiers (0.737)
- **Highest single accuracy**: Elastic Net (BalAcc=0.805) - but requires 2,168 features and poor generalisation to non-linear classifiers
- **Worst performer**: mRMR - redundancy minimisation ineffective when features are weakly correlated

### Reasons underpinning these results

The dataset exhibits **distributed weak signals** with low inter-feature correlation (avg |ρ| < 0.05). This explains:
- **FSS succeeds**: Greedy AIC minimisation captures features with strongest marginal contributions
- **GB-VI succeeds**: Sequential residual fitting accumulates weak signals effectively
- **mRMR fails**: Redundancy minimisation offers no benefit when features are already independent
- **Lasso underperforms**: Hard L1 sparsity discards weakly informative features that collectively matter

## Evaluation framework

Methods ranked using weighted composite score:
- **Parsimony (K*)**: 45% weight - fewer features preferred
- **Accuracy (BalAcc)**: 45% weight - higher balanced accuracy preferred
- **Speed (Runtime)**: 10% weight - faster computation preferred

Cross-classifier validation performed at each method's optimal K* using: Logistic Regression, Linear SVM, kNN, and Random Forest.

## Project Structure
```
├── cache/                                      # Pre-computed results (.pkl files)
├── data/                                       # Dataset
├── feature_selection_report.pdf                # Full analysis report
└── high_dimensional_feature_selection.ipynb    # Main analysis notebook
```

## Important: Clone the Full Repository

⚠️ **Make sure to clone the complete repository including the `cache/` folder.** 

The cache folder contains pre-computed `.pkl` files that save **10+ hours of runtime**. Without these files, the notebook will recompute everything from scratch (FSS alone takes ~5.4 hours).

## Limitations

- Single train-test split (no repeated CV for stability estimates)
- Coarse hyperparameter grid search
- Basic class imbalance handling (`class_weight=balanced`)
- Performance ceiling of BalAcc = 0.805

## Future Directions

- Repeated cross-validation for stable estimates
- Finer hyperparameter tuning
- Advanced resampling (SMOTE, ensemble methods)
- Feature stability analysis via bootstrapping
- Non-linear models (kernel SVM, neural networks)

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
