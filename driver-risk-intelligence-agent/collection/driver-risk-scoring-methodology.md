> **Sample document for the bootcamp.** Illustrative methodology — adapt to your model.

# Driver Risk Scoring — Methodology (Sample)

## Purpose
The risk score estimates a driver's relative likelihood of being involved in a
preventable incident, so that coaching and resources can be targeted where they
reduce harm the most.

## How the score is built
1. **Exposure normalisation.** Raw event counts (speeding, harsh braking,
   acceleration, phone use, fatigue) are converted to **events per 1,000 km**
   using `KM_DRIVEN`, so high-mileage drivers are compared fairly.
2. **Behaviour weighting.** Normalised behaviours are weighted by their observed
   association with incidents. Typical relative weights:
   - Speeding events — high
   - Phone use — high
   - Harsh braking / acceleration — medium
   - Night driving exposure — medium
   - Fatigue flags — medium
3. **History adjustment.** `PRIOR_INCIDENTS` and low `TENURE_MONTHS` increase the
   score; long incident-free tenure decreases it.
4. **Scaling.** The combined value is scaled to 0–100 and bucketed into bands:
   - **Low** 0–39 · **Medium** 40–69 · **High** 70–100.

## Interpreting a score
- The score is **relative**, not a probability. A High band means "prioritise",
  not "will crash".
- Always read the **top contributing factors** alongside the score — two High
  drivers can need completely different interventions.
- Small `KM_DRIVEN` makes a score unstable; flag low-exposure drivers.

## Good practice
- Re-score monthly and watch the **trend**, not just the level.
- Validate the model regularly and monitor for drift.
- Keep the score explainable: every score should decompose into named factors.
