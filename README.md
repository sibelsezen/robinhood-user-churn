# Robinhood User Churn

Estimate historical churn, evaluate a classifier for retention outreach, and identify the features that contribute most to its ranking.

The analysis covers 5,584 users. Under the primary labeling rule, **18.00% churned**. On 1,117 held-out users, the selected random forest achieved **0.315 average precision** versus **0.180** for the constant-score baseline.

## Churn definition and rate

Churn is at least 28 consecutive calendar days with equity below $10 after previously reaching at least $10. The supplied equity panel records equity at or above $10. We used missing calendar dates after each user's first recorded appearance as a proxy for below-threshold equity. The panel contains no weekend observations, so missing dates can also reflect the reporting calendar.

| Rule | Churned users | Rate |
| --- | ---: | ---: |
| Count qualifying gaps between recorded appearances | 275 of 5,584 | 4.92% |
| Also count trailing gaps of at least 28 days by August 18, 2017 (primary) | 1,005 of 5,584 | 18.00% |

Counting qualifying trailing gaps adds 730 users, a difference of 13.07 percentage points based on the unrounded rates. Trailing gaps shorter than 28 days have unresolved outcomes beyond the observation window.

## Model and threshold

We used a stratified 80/20 user split and five-fold stratified cross-validation on the training set. Preprocessing was fitted inside each fold. Both models used one-hot encoding for categorical predictors. Logistic regression scaled numeric inputs, and the random forest used their original scale.

The tuned random forest reached mean validation AP of 0.3130 versus 0.2811 for tuned logistic regression. The selected forest used a maximum depth of 5 and a minimum of 20 samples per leaf, with no class weighting or oversampling.

We chose **0.175** using out-of-fold training scores for an illustrative low-cost in-app reminder. It caught 66.5% of development churners while flagging 46.6% of users. A cutoff of 0.150 had higher F2 (0.529 versus 0.505) and would have flagged 64.2% of users. The choice balances churn coverage and outreach volume under the assumed campaign.

## Held-out evaluation

| Metric | Majority-class baseline | Random forest at 0.175 |
| --- | ---: | ---: |
| Average precision | 0.180 | 0.315 |
| Precision | Undefined (zero flagged) | 26.1% |
| Recall | 0.0% | 69.2% |
| F2 | 0.000 | 0.520 |
| Accuracy | 82.0% | 59.3% |
| Users flagged | 0 of 1,117 | 532 of 1,117 (47.6%) |

| Observed outcome | Unflagged | Flagged |
| --- | ---: | ---: |
| Did not churn | 523 | 393 |
| Churned | 62 | 139 |

The forest flagged 139 of 201 churners. The majority-class baseline caught zero churners.

## Feature importance

Each original feature was shuffled 30 times on the held-out set before preprocessing. The ranking uses the mean drop in average precision and reflects the fitted model's reliance on each feature.

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

These results reflect predictive associations. Estimating the effect of changing a feature requires a separate study.

## Recommendation

The historical ranking merits further validation for a low-cost reminder. Before a pilot, define when users will be scored and verify that every input, especially time spent, is available at that point. Validate the model on a later cohort with a defined prediction window. If the ranking and outreach volume are suitable, randomly assign eligible flagged users to reminder and control groups and compare churn rates.

## Repository and data

- [Data audit notebook](notebooks/01_data_audit.ipynb)
- [Local data placement](data/README.md)

The analysis uses `features_data.csv` and `equity_value_data.csv`. Keep these files and user-level outputs outside Git. The results above are from the completed local analysis. End-to-end reproduction instructions are pending synchronization and verification of the completed notebook.
