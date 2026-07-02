> **Sample document for the bootcamp.** The profiling method this agent follows — edit to your standards.

# Data Profiling & Quality Checklist

Work through these dimensions for every table before it is used in analysis
or modelling. Quantify every finding (a percentage or count, never just a label).

## 1. Structure
- Row count, column count, table size; when was it last updated/loaded?
- Column names, types, labels, formats — do they match the data dictionary?
- Is there a primary key candidate? (e.g. `transaction_id`, `po_number`)

## 2. Completeness
- % missing per column. Flag any key column above 5% and any column above 30%.
- Are missing values true unknowns, or a hidden category (e.g. blank supplier
  for internal transfers)?

## 3. Uniqueness
- Duplicate full rows; duplicate primary keys.
- Near-duplicates that suggest double entry (same supplier + amount + date).

## 4. Validity
- Numeric ranges: negative or zero amounts, quantities, or durations that
  can't be real; absurd maxima (unit price 1000× the median).
- Dates: in the future, before the system existed, or with end < start
  (e.g. contract end before award date).
- Categories: values outside the allowed set (e.g. unknown region codes,
  procurement methods not in the GTPL list).

## 5. Consistency
- Cross-field logic: award value vs. sum of line items; awarded supplier must
  appear among bidders; contract value within budget line.
- Category spelling variants ("Ministry of Health" vs "MOH").

## 6. Procurement-specific quick checks
- One supplier winning an implausible share of awards in a category.
- Many awards just below an approval threshold (possible split purchasing).
- Single-bid tenders as a share of all tenders.

## Reporting format
Verdict first (fit for use / usable with fixes / not usable), then the top
issues with numbers, then the fixes to hand to Data Engineering.
