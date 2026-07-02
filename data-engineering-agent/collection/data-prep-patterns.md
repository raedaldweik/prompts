> **Sample document for the bootcamp.** Preparation patterns this agent follows — edit to your standards.

# Data Preparation Patterns (SAS Viya)

## Ground rules
- Never modify a source table in place — write to a new, descriptively named
  table (`<SOURCE>_CLEAN`, `<SOURCE>_FEATURES`) and promote it.
- Verify after every step: row count in vs. out, and a 10-row sample.
- Keep a one-line log of what each step did (rows dropped and why).

## Common patterns

### Deduplication
Prefer explicit keys: `SELECT DISTINCT` on the business key, or a DATA step
with `BY` + `FIRST.` logic when you must pick a survivor row. Report how many
duplicates were removed.

### Joins
- Confirm join keys exist and have compatible types on both sides first.
- After the join, check the row count against expectation (a fan-out means a
  non-unique key on the many side).
- Left-join reference data (supplier master, entity list) onto transactions;
  count unmatched rows and surface them, don't silently drop.

### Type & value fixes
- Dates arriving as text → `input(txt, yymmdd10.)` (state the format used).
- Standardise category variants (e.g. UPCASE + a mapping table) before any
  group-by.
- Impossible values (negative amounts, future dates) → set missing or filter,
  but always count and report what was affected.

### Feature derivation (procurement examples)
- `days_to_award = award_date - tender_open_date`
- `single_bid = (n_bidders = 1)`
- `just_below_threshold = (threshold - award_value between 0 and 0.05*threshold)`
- Supplier aggregates: awards per supplier per category per year; supplier
  share of category spend.

### Aggregation for reporting
Aggregate to the grain the question needs (per supplier, per month, per
entity) into a small table, then hand off — never hand a million rows to a
chart.

## Synthetic data
Propose the schema first (column, type, range/distribution/categories) and
a row count; generate only after agreement; then preview 5 rows and state the
promoted table name.
