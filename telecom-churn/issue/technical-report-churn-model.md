# Technical Report: Telco Churn Model

Date: 2026-03-15
Project: telecom-churn

## 1) Objective
Build a churn prediction model and document technical findings, risks, and implementation actions.

## 2) Data and Setup
- Dataset size: 7,043 rows, 21 columns
- Target: `Churn` (Yes/No)
- Class distribution:
  - No: 5,174
  - Yes: 1,869
- Train/test split: 80/20 with `stratify=y`

## 3) Root Cause of Initial Failure
### Error
`ValueError: could not convert string to float: '4223-BKEOR'`

### Why it happened
`RandomForestClassifier` requires numeric input. The feature matrix still contained string values (notably `customerID`, categorical text fields, and `TotalCharges` loaded as text).

### Fix applied
1. Dropped `customerID`
2. Converted `TotalCharges` to numeric (`errors='coerce'`)
3. Imputed missing `TotalCharges` with median
4. One-hot encoded categorical columns
5. Re-trained model successfully

## 4) Model Performance (Current Baseline)
- Accuracy: 78.64%
- Classification report:
  - Class `No`: Precision 0.83, Recall 0.89, F1 0.86
  - Class `Yes`: Precision 0.62, Recall 0.49, F1 0.55

### Confusion matrix (Actual x Predicted)
- TN = 924
- FP = 111
- FN = 190
- TP = 184

Interpretation:
- Model is acceptable for identifying non-churners.
- Model misses many churners (`FN=190`), which is high for retention use cases.

## 5) Feature Signals (Top 10)
Top importance signals include:
- `TotalCharges`
- `tenure`
- `MonthlyCharges`
- `PaymentMethod_Electronic check`
- `InternetService_Fiber optic`
- `Contract_Two year`

Note: Feature importance from tree models indicates contribution to split quality, not causal impact.

## 6) Technical Risks
1. Class imbalance lowers churn recall.
2. Single-threshold classification (`0.5`) is not optimized for business cost.
3. No pipeline artifact yet for consistent training/inference.
4. No cross-validation or hyperparameter search in current baseline.
5. No calibration/monitoring design yet.

## 7) Technical Action Plan
## Phase A (Immediate)
- Build a reproducible preprocessing+model pipeline (`ColumnTransformer` + `Pipeline`).
- Use `class_weight='balanced'` baseline.
- Optimize decision threshold for churn recall/retention ROI.
- Track metrics: recall for churn class, precision, F1, PR-AUC, ROC-AUC.

## Phase B (Short Term)
- Run cross-validation.
- Compare models: RandomForest, XGBoost/LightGBM, Logistic Regression baseline.
- Hyperparameter tuning with validation folds.
- Add probability calibration (`CalibratedClassifierCV`) if probability quality is needed.

## Phase C (Production Readiness)
- Persist model + encoder with versioning.
- Add input schema checks and dtype assertions.
- Add drift monitoring (feature drift + churn-rate drift).
- Add monthly retraining policy and performance guardrails.

## 8) Recommended Acceptance Targets
- Churn recall: >= 0.70
- Churn precision: >= 0.55
- PR-AUC: improve over baseline by >= 15%
- Stable performance across cross-validation folds

## 9) What to do next (concrete)
1. Refactor notebook logic into training script in `scripts/`.
2. Add model pipeline and threshold tuning.
3. Retrain and produce metrics table (baseline vs tuned).
4. Publish final model card with business threshold recommendation.
