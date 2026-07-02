> **Sample document for the bootcamp.** Edit to match your real table before the session.

# Procurement Transactions — Data Dictionary

Table: `Public.PROCUREMENT_TRANSACTIONS` (one row per awarded transaction /
purchase order). This is the suggested synthetic schema for the bootcamp —
generate it with the co-pilot's `generate_synthetic_data`.

| Column | Type | Meaning |
|---|---|---|
| `transaction_id` | id | Unique transaction / PO identifier |
| `tender_id` | id | Parent tender (several transactions can share one) |
| `entity` | category | Government entity making the purchase |
| `category` | category | Procurement category (IT, construction, medical, services, …) |
| `region` | category | Region of delivery (Riyadh, Makkah, Eastern, …) |
| `supplier_id` | id | Awarded supplier |
| `supplier_age_years` | float | Years since supplier registration |
| `procurement_method` | category | public_tender, limited_tender, direct_award, framework |
| `tender_open_date` | date | Tender publication date |
| `award_date` | date | Award decision date |
| `days_to_award` | int | `award_date - tender_open_date` |
| `n_bidders` | int | Number of bids received |
| `single_bid` | bool | 1 when `n_bidders = 1` |
| `estimated_value` | float | Pre-tender estimated value (SAR) |
| `award_value` | float | Awarded value (SAR) |
| `price_vs_estimate` | float | `award_value / estimated_value` |
| `near_threshold` | bool | Award value within 5% below an approval threshold |
| `supplier_share_cat` | float | Supplier's share of this category's awards (trailing year) |
| `amendments_count` | int | Post-award contract amendments |
| `amendment_value_pct` | float | Total amendment value as % of original award |
| `is_flagged` | bool | Target: flagged for integrity review (demo label) |

## Reading guidance
- Monetary values are SAR.
- `is_flagged` is the modelling target for the bootcamp — in the demo it is
  synthetic and correlated with the red-flag features by construction.
- Typical grain checks: `transaction_id` unique; `award_date >= tender_open_date`.
