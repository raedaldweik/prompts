# SAS Viya Co-Pilot — Setup

A "co-build" data-science agent backed by the **full SAS MCP server**
(`sas-mcp-server`), which exposes a broad SAS Viya surface (data discovery,
SQL/PROC execution, charts, AutoML, scoring, data insights, **synthetic data
generation**, batch jobs). This is **Pattern A** — one agent, all tools.

> **Heads-up:** this server exposes ~28 tools. That breadth is powerful but
> makes the model slower and occasionally pick a less-ideal tool — the LLM
> re-reads every tool schema on every turn. The system prompt and the
> `sas-viya-tool-guide.md` in the collection keep it on track. For the faster
> alternative, build **Pattern B** — the orchestrated specialists — from the
> same image using `TOOL_GROUPS` slices: see
> [`../docs/orchestration.md`](../docs/orchestration.md). Keep both deployed
> and compare them in the bootcamp.

## 1. Create the collection (RAG)

1. Create a collection named e.g. `data-science-playbook`.
2. Upload the starter documents from [`collection/`](collection):
   - `data-science-project-playbook.md` — the method the agent follows
   - `sas-viya-tool-guide.md` — task → which tool to use (helps tool selection)

## 2. Attach the SAS MCP tool server

Host `sas-mcp-server` (Container MCP Server). Minimum environment:

```bash
VIYA_ENDPOINT=https://<your-viya>
VIYA_REFRESH_TOKEN=<refresh token>     # or VIYA_USERNAME / VIYA_PASSWORD
MAX_SAS_OUTPUT_CHARS=12000             # keep tool output from overflowing context
MCP_API_KEY=<set a key and give it to RAM>
# Leave TOOL_GROUPS unset — the co-pilot gets everything.
```

Performance features (session reuse, connection pooling, fast polling) are
**on by default** — see the repo's *Performance* section for the knobs
(`COMPUTE_SESSION_REUSE`, `HTTP_CLIENT_POOL`, `JOB_POLL_INITIAL`).

You can optionally scope it with `ALLOWED_TABLES` etc. (same as the use-case
server) to keep a bootcamp project focused.

## 3. Create the agent

1. Attach the `data-science-playbook` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the SAS MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder.

## 4. Retrieval settings

- **Top K:** 4–6 (the guide docs are short and specific).

## 5. Test in the NCGR RAM UI

- "Make me a procurement integrity dataset of 5,000 transactions." (agent
  proposes a schema → you confirm → it generates a CAS table with
  `generate_synthetic_data`)
- "What tables are available and what's in them?"
- "Profile PROCUREMENT_TRANSACTIONS — row count, columns, and a chart of
  award value by category."
- "Build a model to predict is_flagged from PROCUREMENT_TRANSACTIONS and tell
  me the champion and its metrics."
- "Which factors most drive is_flagged?" (uses `explain_data`)
