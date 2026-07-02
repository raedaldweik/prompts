# Data Engineering Agent — Setup

Cleans, joins, and shapes raw data into model-ready form — and generates
synthetic datasets for demos. Runs a **scoped slice** of the full SAS MCP server.

## 1. Tool server (shared image, scoped slice)

Create a Container MCP Server from `sas-mcp-server` named e.g.
`SAS Viya MCP — Engineering`. Environment (plus the usual
`VIYA_ENDPOINT` / credentials / `SSL_VERIFY`):

```bash
TOOL_GROUPS=data, datamgmt, sas, synth, files, jobs
```

That exposes discovery + `execute_sas_code`, `upload_data`,
`promote_table_to_memory`, `generate_synthetic_data`, the files tools, and
the batch-job tools.

## 2. Collection (RAG)

Create `data-engineering-knowledge` and upload:
- [`collection/data-prep-patterns.md`](collection/data-prep-patterns.md)
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the SAS Studio
  User's Guide and CAS Fundamentals PDFs.

## 3. Agent

1. Create agent `Data Engineering`, paste [`system_prompt.md`](system_prompt.md).
2. Experiment (Tools-based): attach the Engineering tool server's tools + the
   collection. Top K: 4–6.
3. Champion → start.

## 4. A2A Agent Card (required for the orchestrator)

- **Description:** Cleans, joins, and shapes raw data into model-ready form
  in SAS Viya — and creates synthetic datasets and loads files on request.
- **Skill — Data preparation:** Joins, filters, deduplicates, fixes types,
  and derives features with SAS code, producing promoted CAS tables ready
  for modelling.
- **Skill — Data loading & generation:** Uploads CSV data into CAS and
  generates synthetic datasets from an agreed column specification.
- **Query examples:**
  - Clean the procurement transactions table and remove duplicates.
  - Join the transactions to the supplier master and derive a days-to-award column.
  - Upload this CSV into CAS.
  - Create a synthetic procurement dataset with 5,000 transactions.
  - Aggregate awards by supplier and month into a new table.

## 5. Test

- "Create a synthetic procurement integrity dataset (5,000 rows): tender id,
  supplier, category, region, award value, number of bidders, days to award,
  single-bid flag, flagged outcome." (agent proposes schema → you confirm)
- "Deduplicate PROCUREMENT_TRANSACTIONS on transaction_id into PROC_TXN_CLEAN
  and promote it."
