# Chronic Kidney Disease Prediction Model

A comparative analysis of machine learning models for predicting Chronic Kidney Disease (CKD) using clinical biomarkers. Built in R using the `caret` framework.

## Overview

This project builds and evaluates multiple classification models to predict CKD status from patient lab data. The pipeline includes data cleaning, feature engineering, dimensionality reduction (PCA), and model comparison across Logistic Regression, Random Forest, SVM, and an Ensemble approach.

## Dataset

- **Source:** CKD dataset with 200 patient records
- **Features:** 28 clinical biomarkers including GRF, blood pressure, hemoglobin, blood glucose, serum creatinine, and more
- **Target:** Binary classification — `ckd` vs `notckd`

## Pipeline

1. **Data Cleaning** — Type conversion, range-to-numeric transformation, stage-based median imputation for missing GRF values
2. **EDA** — Distribution plots, boxplots, pairwise analysis
3. **Outlier Detection** — IQR-based analysis across continuous variables
4. **Collinearity Analysis** — Correlation matrix; removed `pcv` (correlated with `hemo`) and `sc` (correlated with `bu`)
5. **Feature Selection** — Removed `affected` (data leakage) and `stage` (derived from GRF)
6. **Chi-Square Tests** — All categorical features significantly associated with class (p < 0.05)
7. **Normalization** — Box-Cox transformation + centering + scaling
8. **PCA** — 10 components retain 95% of variance in continuous features
9. **Modeling** — 75/25 train-test split with 10-fold cross-validation

## Model Results

| Model | Accuracy | F1 Score |
|---|---|---|
| Logistic Regression | 94% | 0.95 |
| Random Forest | 92% | 0.94 |
| SVM (Linear) | 92% | 0.93 |
| SVM Radial (Tuned) | 94% | 0.95 |
| **Ensemble (Majority Vote)** | **94%** | **0.95** |

> Logistic Regression and Tuned SVM Radial both achieved the highest test accuracy. The ensemble model combines predictions via majority voting across all three models.

## Key Findings

- **GRF** is the strongest predictor — clear negative correlation with CKD stage
- **HTN** and **DM** show the strongest association with CKD class (chi-square: 69.4 and 58.2)
- All continuous features follow non-normal distributions (Shapiro-Wilk p < 0.05), justifying Box-Cox transformation
- SVM with radial kernel (C=10, σ=0.01) outperformed linear SVM after hyperparameter tuning

## R Dependencies

```r
install.packages(c("caret", "randomForest", "e1071", "pROC",
                   "ipred", "glmnet", "corrplot", "ggplot2"))
```

## Usage

1. Open `CKD.Prediction.Model.Rmd` in RStudio
2. Install dependencies listed above
3. Knit the document — the dataset is loaded automatically from a hosted URL

## Author

**Bhagyesh Vaze**
