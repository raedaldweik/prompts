> **Sample document for the bootcamp.** The modelling method this agent follows — edit to your standards.

# AutoML Methodology (SAS Viya)

## Before building
- Confirm the target: name, type (binary / nominal / interval), and — for
  binary — which level is the event. State the base rate.
- Confirm the features: drop identifiers and anything unavailable at scoring
  time (leakage — e.g. investigation outcome fields when predicting flags).
- Sanity-check volume: a few thousand labelled rows is a demo; say so.

## Project setup (MLPA)
- `prediction_type`: `binary` for two-level targets (set `target_event_level`),
  `nominal` for 3+ classes, `interval` for numeric.
- Unique, descriptive project names (dataset + purpose + suffix); track the
  returned project id and act by id.
- AutoML partitions data automatically; report which metric the champion was
  selected on (e.g. KS, AUC, misclassification).

## Reading the results
- Champion + challengers: report the leaderboard briefly, not just the winner.
- For imbalanced targets (fraud/flags are usually <10%), prefer AUC/KS and
  precision-recall over accuracy, and say what the confusion trade-off means
  operationally (missed risky awards vs. false alarms for investigators).
- Top predictors: relate them to business meaning (e.g. `single_bid` and
  `days_to_award` dominating suggests process red flags drive risk).

## Limitations to state every time
- Synthetic or small data → metrics are illustrative, not production evidence.
- Class imbalance and how it was handled.
- Any features that risk unfairness (avoid protected characteristics; prefer
  behavioural/process features).

## From model to use
- Register the champion; publishing makes it scoreable in real time
  (MAS module) — that is what the Procurement Integrity use-case agent calls
  via `score_data`.
- Recommend a monitoring plan: score distribution drift, input drift, and a
  periodic retrain trigger.
