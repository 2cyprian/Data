# Telecom Churn Prediction Project

This project predicts customer churn (whether a customer will leave) using supervised machine learning.

## Project Goal
Help business teams identify at-risk customers early and improve retention campaign ROI.

## Project Structure
- `data/` — raw dataset (`telco-churn.csv`)
- `notebooks/` — analysis and model development notebook
- `scripts/` — place for reusable training/inference scripts
- `configs/` — configuration files for reproducible runs
- `issue/` — issue documentation, technical report, and board report

## Tech Stack and Why It Is Used
- **Python**: core language for data and ML workflows.
- **Pandas**: tabular data loading, cleaning, and feature preparation.
- **NumPy**: numerical operations.
- **Scikit-learn**: model training, evaluation metrics, and split utilities.
- **Matplotlib / Seaborn**: model/result visualization (class balance, confusion matrix, feature importance).
- **Jupyter Notebook**: iterative experimentation, debugging, and transparent analysis.

## Model Used
### `RandomForestClassifier`
A tree-ensemble classification model.

### Why this model
- Handles mixed feature patterns well after encoding.
- Captures non-linear relationships.
- Robust baseline with low tuning effort.
- Provides feature importance for explainability.

### Current limitation
With current baseline settings, churn-class recall is lower than desired for retention use cases. This means some true churners are missed.

## Data Preparation Logic (Critical)
Before training, preprocessing must ensure all model inputs are numeric:
1. Remove identifier columns (e.g., `customerID`).
2. Convert numeric-like text fields (e.g., `TotalCharges`) to numeric.
3. Impute missing values.
4. One-hot encode categorical columns.
5. Split train/test with stratification on target.

## Why This Matters (Business Importance)
- Better churn detection supports proactive customer retention.
- Higher recall on churners reduces preventable revenue loss.
- Model outputs can prioritize contact lists for CRM teams.
- Feature importance helps align actions with churn drivers.

## Recommended Next Steps
1. Move notebook logic into versioned scripts in `scripts/`.
2. Use `Pipeline` + `ColumnTransformer` for production-safe preprocessing.
3. Tune threshold and class weighting for higher churn recall.
4. Track KPIs: recall (churn class), precision, F1, PR-AUC, ROI.
5. Add monitoring for drift and monthly retraining.

## Reports
- Technical report: `issue/technical-report-churn-model.md`
- Board report: `issue/board-report-churn-actions.md`
