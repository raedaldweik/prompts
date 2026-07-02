> **Sample document for the bootcamp.** A condensed red-flag playbook drawing on OECD bid-rigging guidance and World Bank procurement-fraud handbooks (see `docs/rag-documents.md` for the full sources). Edit to your methodology.

# Procurement Integrity — Red-Flag Playbook

A red flag is an **indicator that warrants review**, not proof of wrongdoing.
Always report flags with their base rates and the next investigative step.

## 1. Competition red flags
- **Single-bid awards** — especially repeated single-bid wins by the same
  supplier in a competitive category. Check: was the tender advertised long
  enough? Were specs written so only one supplier could qualify?
- **Low bidder turnout** in a category that normally attracts many bidders.
- **Rotating winners / stable market shares** among a fixed group of
  suppliers — a classic bid-rigging pattern (OECD).
- **Losing bids clustered just above the winner** (complementary/cover bids).

## 2. Threshold & splitting red flags
- **Awards just below approval thresholds** (`near_threshold`) — repeated
  awards at 90–99% of a threshold suggest split purchasing to avoid scrutiny.
- **Several same-supplier, same-entity awards within days** that together
  exceed a threshold.

## 3. Process-speed red flags
- **Very short `days_to_award`** for complex categories — evaluation can't
  have been thorough; or the outcome was known in advance.
- **Very long, then amended** — award dragged, then scope/value changed
  post-award.

## 4. Price red flags
- **`price_vs_estimate` far above 1** (overpricing) or **far below**
  (unrealistic bid, expect recovery via amendments).
- **Post-award growth**: `amendment_value_pct` above ~10–15%, or repeated
  amendments (`amendments_count`), turning a competitive award into a
  negotiated one.

## 5. Supplier red flags
- **New suppliers winning large awards** (`supplier_age_years` low, high
  `award_value`) without track record.
- **Supplier concentration**: `supplier_share_cat` high — one supplier
  dominating a category or an entity's spend.
- Shared addresses/owners between "competing" bidders (needs registry data —
  out of scope for the demo table, flag for investigators).

## Investigation next steps (what to recommend)
1. Pull the full transaction set for the supplier/entity/category involved.
2. Compare against peers (same category, same period).
3. Check the tender documents: specs, advertising period, evaluation record.
4. Refer to the integrity/audit team with the quantified pattern — include
   the transactions list, the period, and the rule or guidance implicated.
