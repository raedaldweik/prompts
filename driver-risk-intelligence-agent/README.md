# Driver Risk & Behavioural Intelligence Agent — Setup

This agent uses the **SAS Use-Case MCP** (`SAS_Use_Case` repo) to query the
driver-risk data, chart it, score drivers against ready models, and build models
with AutoML — plus a **collection** of traffic-safety knowledge so it can turn
risk numbers into grounded recommendations.

## 1. Prepare the data in SAS Viya
1. Upload your driver risk score table to CAS (e.g. `Public.DRIVER_RISK_SCORE`).
2. (Optional) Publish a scoring model/decision if you want real-time scoring.

## 2. Create the collection (RAG)
1. Create a collection named e.g. `driver-risk-knowledge`.
2. Upload the starter documents from [`collection/`](collection):
   - `driver-risk-data-dictionary.md` — what each column means
   - `driver-risk-scoring-methodology.md` — how the risk score is built
   - `traffic-safety-interventions.md` — the intervention playbook
3. Edit the data dictionary to match your real table columns.

## 3. Attach the SAS Use-Case MCP tool server
Host the `SAS_Use_Case` server (Container MCP Server) and set its environment.
The key variables (see that repo's `.env.sample`) for this agent:

```bash
VIYA_ENDPOINT=https://<your-viya>
VIYA_REFRESH_TOKEN=<refresh token>     # or VIYA_USERNAME / VIYA_PASSWORD

# Scope the agent to just the driver-risk data + models:
USE_CASE_NAME=Driver Risk & Behavioural Intelligence
ALLOWED_TABLES=Public.DRIVER_RISK_SCORE
ALLOWED_MODELS=<your published risk model name/id>      # optional
ALLOWED_DECISIONS=<your decision name/id>               # optional
SCOPE_ENFORCE=true

MCP_API_KEY=<set a key and give it to RAM>
```

The first entry in `ALLOWED_TABLES` is the **primary table** the tools default
to, so the agent stays focused on driver risk.

## 4. Create the agent
1. Attach the `driver-risk-knowledge` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the SAS Use-Case MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder so the
   traffic-safety guidance is injected.

## 5. Retrieval settings
- **Top K:** 5–8.

## 6. Test in Roads_RAM_UI
- "Show me the distribution of risk scores." (expect a chart)
- "Who are the top 10 highest-risk drivers and why?"
- "Score a driver with [attributes] and recommend what to do."
- "Build a model to predict whether a driver will have an incident next quarter."
