# Data Dictionary — ATS Dataset v2

**Source file:** `Dataset_ATS_v2.csv`  
**Processed outputs:** `processed_full.csv`, `processed_scaled.csv`, `train.csv`, `test.csv`  
**Rows:** 7,043 | **Target:** `Churn`  
**Domain:** Telecom customer churn prediction  
**Prepared by:** Data Engineering  
**Date:** 2026-06-17

---

## Business Context

This dataset captures telecom subscriber attributes and whether they churned (cancelled service). The two analytical tasks it supports are:

1. **Classification** — Predict whether a customer will churn (`Churn` = Yes/No)
2. **Regression** — Predict customer tenure or total spend (`tenure`, `TotalCharges`)

Churn rate in this dataset is **26.5%** (1,869 churned / 5,174 retained). This is a moderately imbalanced classification problem — analysts should not rely on accuracy alone.

---

## Original Columns

| Column | Encoded Name | Type | Values | Business Meaning |
|---|---|---|---|---|
| `gender` | `gender` | Binary int | 1 = Male, 0 = Female | Customer gender — used for demographic segmentation |
| `SeniorCitizen` | `SeniorCitizen` | Binary int | 1 = Senior, 0 = Not | Whether the customer is aged 65+. Seniors show distinct churn patterns |
| `Dependents` | `Dependents` | Binary int | 1 = Yes, 0 = No | Whether the customer has dependents. Customers with dependents tend to be more stable |
| `tenure` | `tenure` | Numeric (int) | 0–72 months | Months the customer has been with the company. The single strongest inverse predictor of churn |
| `PhoneService` | `PhoneService` | Binary int | 1 = Yes, 0 = No | Whether the customer has a phone line. More services = higher switching cost |
| `MultipleLines` | `MultipleLines` | Binary int | 1 = Yes, 0 = No | Whether the customer has multiple phone lines |
| `InternetService` | `internet_fiber` | Binary int (one-hot) | 1 = Fiber optic, 0 = DSL | Internet service tier. Fiber optic customers churn at higher rates, possibly due to competition or price sensitivity |
| `Contract` | `Contract` | Ordinal int | 0 = Month-to-month, 1 = One year, 2 = Two year | Contract commitment level. **Strongest single predictor of churn** — month-to-month customers have no exit barrier |
| `MonthlyCharges` | `MonthlyCharges` | Numeric (float) | 18–119 | Monthly bill amount. High charges combined with short tenure is a key churn risk signal |
| `Churn` | `Churn` | Binary int (target) | 1 = Churned, 0 = Retained | Whether the customer left the company. **Classification target.** |

---

## Engineered Features

| Column | Type | Derivation | Business Meaning |
|---|---|---|---|
| `TotalCharges` | Numeric (float) | `tenure × MonthlyCharges` | Proxy for customer lifetime value (CLV). Useful as both a feature and a regression target |
| `TenureGroup_13-24m` | Binary int | `tenure` binned | Customer is in the 13–24 month lifecycle stage (growing) |
| `TenureGroup_25-48m` | Binary int | `tenure` binned | Customer is in the 25–48 month lifecycle stage (maturing) |
| `TenureGroup_49-72m` | Binary int | `tenure` binned | Customer is in the 49–72 month lifecycle stage (loyal) |
| `HighValueCustomer` | Binary int | `MonthlyCharges > 80` | Flags customers generating high monthly revenue. Used to prioritize retention spend |

> **TenureGroup reference group (dropped):** `0–12m` (new customers). When all three TenureGroup flags are 0, the customer is in the new (0–12 month) cohort — the highest-risk segment.

---

## Encoding Reference

| Original Value | Encoded As |
|---|---|
| Yes | 1 |
| No | 0 |
| Male | 1 |
| Female | 0 |
| Month-to-month | 0 |
| One year | 1 |
| Two year | 2 |
| DSL | 0 (internet_fiber = 0) |
| Fiber optic | 1 (internet_fiber = 1) |

---

## Output Files

| File | Description | Use For |
|---|---|---|
| `processed_full.csv` | All 7,043 rows, encoded + engineered, unscaled | EDA, tree-based models (Random Forest, XGBoost, Decision Tree) |
| `processed_scaled.csv` | Same as above but `tenure`, `MonthlyCharges`, `TotalCharges` are StandardScaled | Linear models, SVM, KNN, Neural Networks |
| `train.csv` | 80% stratified split (5,634 rows) | Model training |
| `test.csv` | 20% stratified split (1,409 rows) | Model evaluation — **do not touch until final evaluation** |
| `scaler_params.csv` | Mean and std used for scaling | Inverse-transform predictions back to original units |

---

## Modelling Notes for Analysts

### Classification (predicting Churn)
- **Target column:** `Churn`
- **Class distribution:** 73.5% No (0) / 26.5% Yes (1)
- **Recommended metric:** ROC-AUC and F1-score (not accuracy)
- **Class imbalance:** Apply SMOTE or `class_weight='balanced'` on `train.csv` only — never on `test.csv`
- **Top expected predictors:** `Contract`, `tenure`, `MonthlyCharges`, `TotalCharges`, `internet_fiber`

### Regression (predicting tenure or TotalCharges)
- **Target options:** `tenure` (customer longevity) or `TotalCharges` (lifetime revenue)
- **Drop `Churn`** when using as a feature — it would leak the outcome
- **Recommended metric:** RMSE, MAE, R²
- **Use `processed_scaled.csv`** for linear regression, SVR, or KNN regressors
- **Use `processed_full.csv`** for Random Forest Regressor or XGBoost Regressor

---

## Known Limitations

- `MonthlyCharges` is stored as integer (no cents) — minor precision loss
- Dataset has no timestamp — no time-series or cohort analysis is possible
- `gender` has near-zero correlation with churn in most studies — may be dropped after feature importance analysis
- No `CustomerID` column — rows cannot be traced back to source records
