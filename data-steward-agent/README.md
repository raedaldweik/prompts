# Data Steward Agent — Setup

Finds the right data, profiles it, and flags quality issues before you build.
Runs a **scoped slice** of the full SAS MCP server.

## 1. Tool server (shared image, scoped slice)

Create a Container MCP Server from `sas-mcp-server` named e.g.
`SAS Viya MCP — Steward`. Environment (in addition to the usual
`VIYA_ENDPOINT` / credentials / `SSL_VERIFY`):

```bash
TOOL_GROUPS=data, insights, charts
```

That exposes only: `list_cas_servers`, `list_caslibs`, `list_castables`,
`get_castable_info`, `get_castable_columns`, `get_castable_data`,
`explain_data`, `render_chart` (+ `get_use_case`).

## 2. Collection (RAG)

Create `data-steward-knowledge` and upload:
- [`collection/data-profiling-checklist.md`](collection/data-profiling-checklist.md)
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the SAS
  Information Catalog User's Guide and SAS Data Explorer User's Guide PDFs.

## 3. Agent

1. Create agent `Data Steward`, paste [`system_prompt.md`](system_prompt.md).
2. Experiment (Tools-based): attach the Steward tool server's tools + the
   collection. Top K: 4–6.
3. Champion → start.

## 4. A2A Agent Card (required for the orchestrator)

- **Description:** Finds the right data in SAS Viya, profiles it, and flags
  quality issues before anything is built on it.
- **Skill — Data discovery:** Lists CAS libraries and tables and describes
  their structure (columns, types, sizes).
- **Skill — Data profiling & quality:** Profiles tables (missing values,
  duplicates, out-of-range values, distributions) and explains which
  variables drive a target column.
- **Query examples:**
  - What procurement data do we have available?
  - Profile the PROCUREMENT_TRANSACTIONS table.
  - Are there data quality issues in the supplier data?
  - Which columns drive the flagged outcome in this table?
  - Show me the distribution of award values.

## 5. Test

- "What tables are in the Public caslib?"
- "Profile PROCUREMENT_TRANSACTIONS — completeness, duplicates, and a chart
  of transactions by category."
- "Which variables most influence is_flagged?" (uses `explain_data`)
