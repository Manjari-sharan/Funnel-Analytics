# ML Models

This folder contains the baseline modeling notebook for purchase-session classification.

## Baseline Logistic Regression

Notebook:

```text
Baseline Logistic Regression.ipynb
```

Purpose:

- Load the Tableau-ready enhanced session-level dataset.
- Build a simple baseline Logistic Regression model.
- Build an enhanced Logistic Regression model using additional session-level features.
- Evaluate class imbalance using ROC-AUC, PR-AUC, precision, recall, confusion matrix, feature importance, feature ablation, and threshold tuning.

Input:

```text
../Dataset/Processed Dataset/tableau_session_funnel_data.csv
```

Output:

```text
Model evaluation results inside the notebook
```

Note:

If `tableau_session_funnel_data.csv` is missing, run `../Notebooks/Tableau Data Preparation.ipynb` first.

If `funnel_data.csv` is also missing, run `../Notebooks/Funnel Analytics.ipynb` before Tableau Data Preparation.
