# Churn Model Training Issue (Random Forest)

## What issue happened?
While training the model in the notebook (Cell 7), training failed with:

`ValueError: could not convert string to float: '4223-BKEOR'`

## What was wrong?
`RandomForestClassifier` requires numeric features, but `x_train` still had string values:
- `customerID` (identifier text)
- categorical feature columns (text categories)
- `TotalCharges` can be loaded as text in this dataset

Because of this, scikit-learn could not convert all input values to numeric format.

## How was it solved?
In the preprocessing step (Cell 6), the pipeline was corrected to:
1. Drop `customerID`
2. Convert `TotalCharges` to numeric with `errors='coerce'`
3. Fill missing `TotalCharges` with median
4. One-hot encode categorical columns
5. Split data with `stratify=y`

After these changes, model training completed successfully.

## What to do to avoid this next time
Use this checklist before model training:

- Ensure no identifier columns are used as features
- Convert known numeric-like text columns (`TotalCharges`) using `pd.to_numeric`
- Handle missing values after conversion
- Encode categorical columns before fitting scikit-learn models
- Validate dtypes before fit:
  - confirm no `object`/string columns remain in `X`
- Prefer a reusable preprocessing pipeline (`ColumnTransformer` + `Pipeline`) to make training and inference consistent

## Recommended validation snippet

```python
# quick pre-fit checks
print(x_train.dtypes.value_counts())
assert x_train.select_dtypes(include=['object', 'string']).shape[1] == 0, 'Non-numeric columns still exist'
```
