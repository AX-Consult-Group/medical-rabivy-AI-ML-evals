# Medical Rabivy robustness evals

![Infographic: Design Overview](infographic.png) 

**Disclaimer**: 

Rabivy and AX Pharmaceuticals are fictional and were created solely for the purposes of this demonstration and portfolio project. This project is an independent demonstration and was not commissioned by any pharmaceutical company. 

---

### Introduction 

Rabivy is AX Pharmaceutical's obesity asset with a monthly (or less frequent) dosing regimen and cardiometabolic positioning. As a pre-launch asset, no real-world prescribing data yet exists. Commercial analytics teams must therefore develop robust methods to identify which Healthcare Professionals (HCPs) are most likely to prescribe Rabivy at launch and to understand the key drivers of early adoption. The closest benchmarks are the launches of semaglutide (Wegovy) and tirzepatide (Zepbound). 

A covariate-shift robustness evaluation of three frozen classifiers (LR, XGBoost, MLP) predicting HCP prescribing propensity. The data-generating process is held fixed and only covariate distributions shift by state — isolating transportability under covariate shift from concept drift, which real external validation confounds.

The primary objective of this analysis is to build and validate a propensity model that predicts which HCPs are likely to prescribe Rabivy in the first 12 weeks post-launch. 

---

### Data Generation 

Because no real prescribing labels exist, we developed a fully synthetic dataset of 5,000 HCPs. The data-generating process (DGP) was explicitly constructed to reflect realistic clinical and commercial drivers observed in analogous GLP-1 launches. Key features were simulated across prescription behaviour, patient panel characteristics, payer/access dynamics, and rep engagement. A binary outcome (“likely to prescribe”) was generated from a known ground-truth function, allowing rigorous validation that models recover the planted signals.

---

### Modelling Approach 

We evaluated three complementary modelling strategies:

1. Logistic Regression – A transparent, interpretable baseline using main effects.

2. XGBoost – A gradient boosting model that captures non-linear relationships and interactions, with strong explainability via SHAP.

3. Feed-Forward Neural Network (MLP) – A deep learning model implemented in keras3 (TensorFlow) to explore more complex pattern learning.

All models were trained on the same set of engineered features and evaluated on a held-out test set (30%).

---

### Evaluation Framework

To ensure robust and unbiased assessment, we implemented a structured **evaluation harness** using a **golden test set** — a 30% hold-out portion of the data that was completely withheld from model training and hyperparameter tuning. This approach simulates how the models would perform on future, unseen HCPs.

All models were evaluated on the golden test set using a consistent set of metrics:

- Discrimination: AUC and Gini coefficient.

- Balanced performance: Balanced Accuracy, Sensitivity, and Specificity.

- Precision: The proportion of HCPs predicted as likely to prescribe who actually were (Positive Predictive Value).

- Calibration: Brier Score and visual calibration plots (predicted vs. observed rates).

- Signal recovery: Comparison of feature importance against the planted ground-truth drivers from the DGP.

This multi-metric harness provides a comprehensive view of both predictive power and practical usability, allowing confident model selection for commercial deployment.

---

### Data & Documentation

- `hcp_propensity_model.qmd` — Main Quarto report containing simulation, modeling, and evaluation.
- `data/hcp_simulation_data.rds` — Frozen synthetic dataset (5,000 HCPs) used for all analysis.
- `documentation/HCP_Propensity_Codebook.xlsx` — Codebook for engineered features.

---

### How to view the report

The full rendered report is available via **GitHub Pages**: 

https://ax-consult-group.github.io/Medical-Rabivy-robustness-evals/

---

### Technologies used
The analysis was conducted in R using the following packages:

**Core R Packages**
- tidyverse
- xgboost
- pROC
- kableExtra
- caret
- broom
- shapviz
- ROCR

**Deep Learning Packages**
- reticulate
- keras3
- tensorflow
---
