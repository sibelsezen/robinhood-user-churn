# Requirements and acceptance criteria

## 1. Data audit
- Record source, permitted use, files, row counts, columns, types, and join keys.
- Determine each table's grain and user population.
- Inspect date parsing, timezone, observation frequency, per-user coverage, and gaps.
- Check duplicate user-date records, null IDs, missing equity, and invalid values.
- Obtain the data dictionary and reporting conventions before filling missing dates.

## 2. Labels and descriptive rate
- Require equity of at least $10 before a qualifying below-$10 spell.
- Count 28 consecutive calendar days, with an explicitly documented inclusive-day convention.
- Document missing-date handling; do not silently treat missing equity as zero.
- Separate confirmed events, no observed event, ineligible users, and indeterminate follow-up.
- Report the requested all-user percentage with its observation limitations; separately report any eligible/evaluable population rate with its denominator.
- Test exactly $10, 27 versus 28 days, interrupted spells, calendar gaps, never-funded users, recovery, and dataset boundaries.
- Compare defensible interpretations if the data cannot uniquely establish the label.

## 3. Prediction
- Define who is scored, when scoring occurs, and what future event is predicted.
- Use only information available by the prediction cutoff.
- Define feature and outcome windows after checking available history.
- Prevent repeated-user and overlapping-window leakage in validation.
- Fit imputers, encoders, scalers, feature selection, and any resampling on training folds only.
- Compare a simple baseline, logistic regression, and a tree-based model.
- Choose metrics after reviewing class prevalence and business use.
- Assess ranking and probability quality; select thresholds on validation data.
- Report final held-out performance and limitations.

## 4. Interpretation and reproducibility
- Rank features using a documented method and discuss correlated predictors.
- Describe associations without asserting causation.
- Translate findings into retention hypotheses that could be tested.
- Provide environment instructions, scripts, configuration, and tests.
- Record random seeds, dataset version or checksum, and assumptions.
- Keep raw data and user identifiers out of repository history.

## Current status
Only project documentation has been prepared. Data, labels, models, tests, and results are pending.
