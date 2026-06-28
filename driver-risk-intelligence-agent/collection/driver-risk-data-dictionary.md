> **Sample document for the bootcamp.** Edit the columns to match your real `DRIVER_RISK_SCORE` table.

# Driver Risk Score — Data Dictionary (Sample)

Table: `Public.DRIVER_RISK_SCORE`. One row per driver per scoring period.

| Column | Type | Description |
|--------|------|-------------|
| `DRIVER_ID` | string | Unique, pseudonymised driver identifier. |
| `SCORE_PERIOD` | date | The month the score applies to (YYYY-MM-01). |
| `RISK_SCORE` | numeric | Overall risk score, 0–100 (higher = riskier). |
| `RISK_BAND` | string | Derived band: Low (0–39), Medium (40–69), High (70–100). |
| `KM_DRIVEN` | numeric | Kilometres driven in the period (exposure). |
| `SPEEDING_EVENTS` | numeric | Count of speeding events (> threshold over limit). |
| `HARSH_BRAKING` | numeric | Count of harsh-braking events. |
| `HARSH_ACCEL` | numeric | Count of harsh-acceleration events. |
| `NIGHT_KM_PCT` | numeric | Percent of distance driven 22:00–05:00. |
| `PHONE_USE_MIN` | numeric | Minutes of detected handheld phone use while driving. |
| `FATIGUE_FLAGS` | numeric | Count of fatigue-indicator events. |
| `PRIOR_INCIDENTS` | numeric | Incidents in the previous 12 months. |
| `TENURE_MONTHS` | numeric | Months of driving experience on record. |

## Notes
- **Exposure matters:** normalise event counts by `KM_DRIVEN` (events per 1,000 km) before comparing drivers.
- `RISK_BAND` is a convenience derived from `RISK_SCORE`; use the numeric score for ranking.
- Do not use any demographic or protected attribute as a risk factor.
