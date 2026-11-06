# Robinhood User Churn

This project estimates historical churn, evaluates a classifier for retention outreach, and ranks the features that contribute to its performance.

## Churn definition

Churn means equity below $10 for at least 28 consecutive calendar days after previously reaching $10. The equity panel records dates at or above $10. We used gaps after each user's first recorded appearance as a proxy for below-threshold equity. The panel has no weekend observations, so gaps can also reflect the reporting calendar.

| Labeling rule | Users labeled churned | Rate |
| --- | ---: | ---: |
| Qualifying gaps between recorded appearances | 275 of 5,584 | 4.92% |
| Also include trailing gaps that reached 28 days by August 18, 2017 (primary) | 1,005 of 5,584 | 18.00% |

The trailing-gap rule adds 730 users, or 13.07 percentage points using unrounded rates. Shorter trailing gaps have unresolved outcomes beyond the observation window.

## Modeling and results

We used a stratified 80/20 user split and five-fold stratified cross-validation for tuning. We one-hot encoded categorical inputs within each fold and scaled numeric inputs for logistic regression. The tuned random forest achieved mean validation average precision (AP) of 0.3130 versus 0.2811 for tuned logistic regression. The selected forest used depth 5, a minimum of 20 samples per leaf, and no class weighting or oversampling.

For an illustrative low-cost in-app reminder, we selected a score cutoff of 0.175 using out-of-fold training scores. It caught 66.5% of development churners and flagged 46.6% of users. A cutoff of 0.150 had higher F2 (0.529 versus 0.505) and flagged 64.2% of users.

| Held-out metric | Majority-class baseline | Random forest at 0.175 |
| --- | ---: | ---: |
| Average precision | 0.180 | 0.315 |
| Precision | Undefined (zero flagged) | 26.1% |
| Recall | 0.0% | 69.2% |
| F2 | 0.000 | 0.520 |
| Accuracy | 82.0% | 59.3% |
| Users flagged | 0 of 1,117 | 532 of 1,117 (47.6%) |

The forest flagged 139 churners and 393 non-churners. It missed 62 churners and left 523 non-churners unflagged.

## Feature importance

We shuffled each original input 30 times on the held-out set and ranked features by the mean drop in AP.

| Feature | Mean AP drop |
| --- | ---: |
| First deposit amount | 0.0499 |
| Risk tolerance | 0.0441 |
| Time spent | 0.0236 |
| Liquidity needs | 0.0232 |
| Platform | 0.0051 |
| First instrument traded | 0.0046 |
| Investment experience | 0.0036 |
| Time horizon | −0.0027 |

The ranking measures how the fitted model relies on its inputs. Establishing causal effects requires a separate study.

## Recommendation

Before a retention pilot, define the scoring date and outcome window, verify that all features, especially time spent, are available at scoring time, and validate the model on a later cohort. If the ranking and outreach volume are suitable, randomly assign flagged users to reminder and control groups to measure the effect on churn.

## Run the analysis

1. Place the supplied `features_data.csv` and `equity_value_data.csv` in `data/`. Keep both CSVs and user-level outputs outside Git.
2. From the repository root, create an environment with Python 3.11 and start Jupyter.

   ```bash
   python3.11 -m venv .venv
   source .venv/bin/activate
   python -m pip install notebook ipykernel pandas numpy matplotlib scikit-learn
   python -m notebook
   ```

3. Open [the analysis notebook](notebooks/01_data_audit.ipynb) and run all cells in order. Compare the churn counts, test metrics, and feature ranking with the results above.
