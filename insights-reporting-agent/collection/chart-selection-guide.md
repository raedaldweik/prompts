> **Sample document for the bootcamp.** How this agent picks and captions charts — edit to your standards.

# Chart Selection & Executive Summary Guide

## Pick the form from the question
| The question is about… | Use | Notes |
|---|---|---|
| Comparing categories (suppliers, entities, regions) | **bar** | Sort by value; top 10 + "other" |
| Change over time (months, quarters) | **line** | One line per series, max 4 |
| Cumulative volume over time | **area** | Stack only if parts sum to a whole |
| Share of a whole (spend by method) | **pie** | ≤ 6 slices or use a bar instead |
| Relationship between two measures (value vs. bidders) | **scatter** | Point per entity, note outliers |

## Rules of thumb
- **Aggregate first.** Chart the summary (top-N, per-month, per-entity), never
  raw transactions.
- **One message per chart.** If the takeaway needs two sentences, make two
  charts.
- **Always a takeaway line.** "X is concentrated in Y" — the reader should get
  the point without reading the axes.
- **Name the source.** Table, filter, and date range under the summary.

## Procurement examples
- "Top suppliers by award value" → bar, sorted, top 10.
- "Single-bid awards over time" → line by month; add a second line for total
  awards to show the share is rising, not just volume.
- "Award value vs. number of bidders" → scatter; annotate the high-value,
  single-bid corner — that's the red-flag quadrant.

## Executive summary skeleton
1. **Headline** — the decision-relevant finding in one sentence.
2. **Evidence** — 2–4 bullets, each a number or chart reference.
3. **So what** — the recommended action or decision.
4. **Source** — data, filters, date range, and any caveats (synthetic/demo data!).
