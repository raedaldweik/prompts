# Model Builder Agent — Setup

Builds, compares, and scores ML models with SAS AutoML in minutes. Runs a
**scoped slice** of the full SAS MCP server.

## 1. Tool server (shared image, scoped slice)

Create a Container MCP Server from `sas-mcp-server` named e.g.
`SAS Viya MCP — Models`. Environment (plus the usual
`VIYA_ENDPOINT` / credentials / `SSL_VERIFY`):

```bash
TOOL_GROUPS=data, ml, score, jobs
```

That exposes discovery + the AutoML tools (`create_ml_project`,
`run_ml_project`, `list_ml_projects`, `delete_ml_project`,
`list_registered_models`), scoring (`list_models_and_decisions`,
`score_data`), and the batch-job tools.

## 2. Collection (RAG)

Create `model-builder-knowledge` and upload:
- [`collection/automl-methodology.md`](collection/automl-methodology.md)
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the SAS Viya
  Machine Learning (Model Studio) User's Guide and SAS Model Manager User's
  Guide PDFs.

## 3. Agent

1. Create agent `Model Builder`, paste [`system_prompt.md`](system_prompt.md).
2. Experiment (Tools-based): attach the Models tool server's tools + the
   collection. Top K: 4–6.
3. Champion → start.

## 4. A2A Agent Card (required for the orchestrator)

- **Description:** Builds, compares, and scores machine-learning models with
  SAS AutoML, and manages registered and published models.
- **Skill — Model building (AutoML):** Creates and runs AutoML projects on a
  CAS table for a given target variable and reports the champion model with
  honest validation metrics.
- **Skill — Real-time scoring:** Lists published models and decisions and
  scores individual records against them, explaining the result.
- **Query examples:**
  - Build a model to predict flagged transactions in PROC_TXN_CLEAN.
  - Which model is the champion and how good is it really?
  - Score this transaction against the procurement risk model.
  - What models are already registered for procurement?
  - Retrain the procurement risk project.

## 5. Test

- "Build a model on PROC_TXN_CLEAN predicting is_flagged (binary, event
  level 1) — name it clearly and report the champion."
- "Score a transaction: award value 4.9M, single bidder, 3 days to award."
