# AI & Data Science Agent — Setup

A "co-build" data-science agent backed by the **full SAS MCP server**
(`sas-mcp-server`), which exposes a broad SAS Viya surface (data discovery,
SQL/PROC execution, charts, AutoML, scoring, data insights, batch jobs).

> **Heads-up:** this server exposes ~27 tools. That breadth is powerful but can
> make the model slower or occasionally pick a less-ideal tool. The system
> prompt and the `sas-viya-tool-guide.md` in the collection are designed to keep
> it on track by giving it a clear workflow and a task→tool map. If a given
> bootcamp project only needs a few capabilities, consider using the focused
> `SAS_Use_Case` server instead.

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
```

You can optionally scope it with `ALLOWED_TABLES` etc. (same as the use-case
server) to keep a bootcamp project focused.

## 3. Create the agent
1. Attach the `data-science-playbook` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the SAS MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder.

## 4. Retrieval settings
- **Top K:** 4–6 (the guide docs are short and specific).

## 5. Test in Roads_RAM_UI
- "What tables are available and what's in them?"
- "Profile the [table] — row count, columns, and a chart of [metric] by [dimension]."
- "Build a model to predict [target] from [table] and tell me the champion and its accuracy."
- "Which factors most drive [target] in [table]?" (uses `explain_data`)
