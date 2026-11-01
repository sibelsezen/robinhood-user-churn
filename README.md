# Robinhood User Churn
Reproducible analysis of user churn, prediction of churn probabilities, and identification of associated user behaviors.

**Status:** Project setup. Dataset inspection and implementation are pending. No results or model performance claims are available yet.

## Business objective
Support retention decisions by answering:
1. What percentage of users in the data have churned?
2. Can future churn be predicted, and how well does the model perform?
3. Which features are most associated with churn?

This is an independent educational case study, not an official Robinhood product or analysis. Dataset provenance will be documented after inspection.

## Churn definition
A user has churned when their equity value stays below $10 for at least 28 consecutive calendar days, having previously been at least $10.

Exactly $10 satisfies the prior-equity condition but does not count as below $10. Calendar days must not be replaced with row counts without verifying daily coverage.

### Decisions to resolve from the data
- Observation frequency, gaps, duplicate user-date records, and timezone.
- Whether missing dates represent unobserved equity or a documented reporting convention.
- Eligibility of users who never reach $10.
- Incomplete follow-up and outcomes that cannot be determined.
- First churn event, subsequent recovery, and the analysis period.
- Denominator for the requested all-user percentage versus an eligible-user rate.
- Prediction date, feature history, outcome horizon, and split strategy.

Assumptions will be recorded explicitly and sensitivity analysis used where alternatives are defensible. Low equity is the supplied churn proxy; it does not by itself prove a withdrawal.

## Implementation roadmap
| Stage | Deliverable |
| --- | --- |
| Data audit | Schema, date coverage, missingness, duplicates, data dictionary |
| Labeling | Documented rules, edge-case tests, per-user labels |
| Descriptive analysis | Churn counts, rates, population definitions, exclusions |
| Prediction design | Historical feature window, future outcome window, leakage controls |
| Modeling | Simple baseline, logistic regression, tree-based comparison |
| Evaluation | Held-out discrimination, probability quality, threshold tradeoffs |
| Interpretation | Feature associations, limitations, retention hypotheses |
| Reproducibility | Reusable scripts, dependencies, tests, execution instructions |

Model selection and threshold tuning will use training/validation data only. The final test set will remain untouched until final evaluation. Preprocessing will be fitted on training data only. Repeated users and overlapping outcome windows will be considered when designing splits.

## Project layout
- `docs/`: requirements, assumptions, and methodology.
- `data/`: local data placement guidance; datasets are not committed.
- `notebooks/`: explanatory exploration, to be added after data inspection.
- `src/`: reusable audit, labeling, feature, training, and evaluation code, to be added.
- `tests/`: labeling and pipeline checks, to be added.
- `reports/`: generated analysis and figures, to be added.

## Getting started
Implementation commands and dependency versions will be added once the dataset format has been inspected. This repository does not yet contain an executable analysis.

Next step: obtain the supplied dataset and accompanying data documentation, then complete the audit before implementing churn labels.

## Expected outputs
- Re-runnable churn labeling and a documented churn-rate calculation.
- Churn probabilities for the defined scoring population.
- Model comparison with clearly stated evaluation windows.
- Feature rankings, limitations, and business recommendations.
- Instructions that allow another analyst to reproduce the work.
