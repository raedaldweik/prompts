> **Sample document for the bootcamp.** A lightweight method the agent should follow.

# Data Science Project Playbook (Sample)

A simple, reliable flow for building an analytics or ML solution with SAS Viya.

## 1. Frame the problem
- State the **business question** and the **decision** it will drive.
- Define the **target** (what you're predicting/measuring) and the **unit of analysis** (per driver? per trip? per day?).
- Agree what "good" looks like (the success metric).

## 2. Understand the data
- Locate the source table(s); check row counts, columns, and types.
- Sample the data and sanity-check ranges, missingness, and duplicates.
- Build a quick profile: distributions of key variables, and the target's balance.

## 3. Prepare the data
- Handle missing values explicitly (impute or exclude — and say which).
- Engineer features that encode domain knowledge (e.g. rates per exposure).
- Avoid **leakage**: never use information that wouldn't be available at prediction time.
- Split into training/validation (and holdout) before modelling.

## 4. Model (AutoML)
- Create an AutoML project with the defined target.
- Run the pipeline; let it compare algorithms.
- Read the **leaderboard** and the **champion** model and its metrics.

## 5. Evaluate honestly
- Report the metric on validation/holdout, not training.
- Check for class imbalance, overfitting, and stability.
- Inspect the most important features — do they make business sense?

## 6. Communicate & operationalise
- Summarise findings for a business audience: what, so what, now what.
- If deploying, publish the model and use real-time scoring.
- Plan monitoring for drift and periodic re-training.

## Guardrails
- Validate every assumption against the actual data before acting.
- Keep steps small and reversible; confirm before overwriting or deleting.
- Prefer explainable results; a model nobody trusts won't be used.
