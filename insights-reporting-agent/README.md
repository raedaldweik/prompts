# Insights & Reporting Agent — Setup

Turns results into clear visuals and decision-ready summaries. Runs a
**scoped slice** of the full SAS MCP server.

## 1. Tool server (shared image, scoped slice)

Create a Container MCP Server from `sas-mcp-server` named e.g.
`SAS Viya MCP — Reporting`. Environment (plus the usual
`VIYA_ENDPOINT` / credentials / `SSL_VERIFY`):

```bash
TOOL_GROUPS=data, charts, insights, sas
```

That exposes discovery + `render_chart`, `explain_data`, and
`execute_sas_code` (for aggregations feeding the charts).

## 2. Collection (RAG)

Create `insights-reporting-knowledge` and upload:
- [`collection/chart-selection-guide.md`](collection/chart-selection-guide.md)
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the SAS Visual
  Analytics *Designing Reports* and *Working with Report Data* PDFs.

## 3. Agent

1. Create agent `Insights & Reporting`, paste [`system_prompt.md`](system_prompt.md).
2. Experiment (Tools-based): attach the Reporting tool server's tools + the
   collection. Top K: 4–6.
3. Champion → start.

## 4. A2A Agent Card (required for the orchestrator)

- **Description:** Turns analysis results into clear interactive charts and
  short decision-ready summaries for leaders.
- **Skill — Charting:** Aggregates data and renders bar, line, area, pie, and
  scatter charts in the chat UI, each with a one-line takeaway.
- **Skill — Executive summaries:** Produces headline-first summaries of
  findings with supporting numbers and a recommended action.
- **Query examples:**
  - Chart award value by supplier for the top 10 suppliers.
  - Show the monthly trend of single-bid awards.
  - Summarise the procurement risk findings for management.
  - What drives flagged transactions? Give me a visual.
  - Make this analysis decision-ready.

## 5. Test

- "Top 10 suppliers by total award value — bar chart with a takeaway."
- "Monthly trend of transactions and flagged transactions this year — line
  chart, then a 3-bullet executive summary."
