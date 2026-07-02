# Procurement Integrity Agent — Setup

The flagship NCGR use case. This agent talks to a **finished** project: it
uses the **SAS Use-Case MCP** (`SAS_Use_Case` repo) to query the procurement
transactions data, chart patterns, and score transactions in real time
against the deployed risk model — plus a **collection** of procurement law
and red-flag guidance so every flag is tied to the rule behind it.

> Typical bootcamp flow: use the **SAS Viya Co-Pilot** first to generate the
> synthetic procurement dataset, clean it, and build + publish the risk model.
> Then point this agent at the results.

## 1. Prepare the data & model in SAS Viya

1. Load (or generate with the co-pilot) the procurement transactions table,
   e.g. `Public.PROCUREMENT_TRANSACTIONS` — suggested schema in
   [`collection/procurement-data-dictionary.md`](collection/procurement-data-dictionary.md).
2. Build the risk model on it (co-pilot / Model Studio) and **publish** it so
   it is scoreable in real time (a MAS module), e.g. `procurement_risk_model`.

## 2. Create the collection (RAG)

Create `procurement-integrity-knowledge` and upload:
- [`collection/procurement-data-dictionary.md`](collection/procurement-data-dictionary.md) — what each column means (edit to match your table!)
- [`collection/procurement-red-flags.md`](collection/procurement-red-flags.md) — the red-flag playbook
- [`collection/gtpl-compliance-notes.md`](collection/gtpl-compliance-notes.md) — bootcamp notes on the procurement law
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the official
  English **Government Tenders and Procurement Law** and **Executive
  Regulations** PDFs (Ministry of Finance), and the OECD bid-rigging
  guidelines.

## 3. Attach the SAS Use-Case MCP tool server

Host the `SAS_Use_Case` server (Container MCP Server) and set its environment:

```bash
VIYA_ENDPOINT=https://<your-viya>
VIYA_REFRESH_TOKEN=<refresh token>     # or VIYA_USERNAME / VIYA_PASSWORD

# Scope the agent to the procurement use case:
USE_CASE_NAME=Procurement Integrity
USE_CASE_DESCRIPTION=Detects integrity risks in government procurement: analyses purchase orders and tender awards, explains red flags, and scores transactions in real time with the deployed procurement risk model.
ALLOWED_TABLES=Public.PROCUREMENT_TRANSACTIONS
ALLOWED_MODELS=procurement_risk_model
ALLOWED_DECISIONS=procurement_risk_scoring       # if you publish a decision
SCOPE_ENFORCE=true

MCP_API_KEY=<set a key and give it to RAM>
```

The first entry in `ALLOWED_TABLES` is the **primary table** the tools
default to, so the agent stays focused on procurement.

## 4. Create the agent

1. Attach the `procurement-integrity-knowledge` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the SAS Use-Case MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder so the law and
   red-flag guidance are injected.

## 5. Retrieval settings

- **Top K:** 5–8 (law + red-flag questions benefit from a bit more context).

## 6. Test in the NCGR RAM UI

- "Show me the distribution of award values." (expect a chart)
- "Which suppliers won the most single-bid awards, and what does the law say
  about direct awards?"
- "Are there signs of split purchasing near approval thresholds?"
- "Score this transaction: award value 4.9M, single bidder, 3 days to award,
  new supplier." (expect a score + red flags + citation)
- "Build a model to predict which transactions get flagged."
