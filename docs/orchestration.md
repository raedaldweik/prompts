# Building the Orchestrated Co-Pilot in SAS RAM

The target architecture (one assistant, five specialists):

```
                       ┌─────────────────────────────┐
                       │      Orchestrator Agent      │
                       │ understands the request and  │
                       │ routes it to the specialist  │
                       └──────────────┬──────────────┘
      ┌──────────────┬───────────────┼───────────────┬──────────────┐
┌─────▼─────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌─────▼─────┐
│   Data    │ │    Data     │ │    Model    │ │  Insights & │ │ Platform  │
│  Steward  │ │ Engineering │ │   Builder   │ │  Reporting  │ │   Guide   │
└─────┬─────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └─────┬─────┘
      │              │               │               │              │
      └────── each agent: a TOOL_GROUPS slice of sas-mcp-server ────┘
                     + its own SAS-documentation collection (RAG)
```

RAM supports this natively — you do **not** need a separate "orchestrator MCP
server". RAM has two agent types: **agents** (tools + collections + LLM) and
**orchestrator agents** (route requests to agents based on their **A2A agent
cards**). Build the five specialists first, then put the orchestrator in front.

## Step 1 — Deploy five tool servers from ONE image

In **MCP Tools**, create five tool servers, all from the same
`ghcr.io/raedaldweik/sas-mcp-server:latest` Container MCP Server template
(Transport HTTP, Port 8134, Base Path /mcp). On each server's Environment
Variables tab set the usual `VIYA_ENDPOINT` / credentials / `SSL_VERIFY`
variables **plus a different `TOOL_GROUPS` slice**:

| Tool server name | `TOOL_GROUPS` |
|---|---|
| SAS Viya MCP — Steward | `data, insights, charts` |
| SAS Viya MCP — Engineering | `data, datamgmt, sas, synth, files, jobs` |
| SAS Viya MCP — Models | `data, ml, score, jobs` |
| SAS Viya MCP — Reporting | `data, charts, insights, sas` |
| *(Platform Guide has no tool server — pure RAG)* | — |

Why slices matter: on every agent turn the LLM re-reads every attached tool's
schema. 28 tools ≈ thousands of prompt tokens and more wrong-tool choices;
6–10 tools is faster and more accurate.

## Step 2 — Create the five collections (RAG)

One collection per specialist. Upload the starter docs from each agent's
`collection/` folder plus the SAS documentation PDFs listed in
[`rag-documents.md`](rag-documents.md) (each specialist gets the docs for its
own products — e.g. Model Builder gets the Model Studio and Model Manager
guides). Vectorize and pick a champion configuration, and **give each
collection a description** — required later for agentic retrieval.

## Step 3 — Create the five specialist agents

For each of the five (READMEs: `data-steward-agent`, `data-engineering-agent`,
`model-builder-agent`, `insights-reporting-agent`, `platform-guide-agent`):

1. **Agents → Create an Agent**, name it, paste its `system_prompt.md`.
2. Add an **agent experiment** (Tools-based): select the LLM + retrieval
   setting, tick only the tools from its tool server, attach its collection.
3. Evaluate → select as **champion** → **start** the agent.
4. Fill in the **A2A Agent Card** (description, skills, query examples — each
   README has a copy-paste block). The orchestrator routes ONLY by what's on
   these cards, so the query examples matter more than anything else.

## Step 4 — Create the orchestrator

1. **Agents → Create an Agent Orchestrator**, name it e.g.
   `NCGR SAS Viya Co-Pilot`, paste `orchestrator-agent/system_prompt.md`.
2. Add an experiment. Two options:
   - **Basic no-code experiment** — pick the LLM, then on the **Agents** tab
     select the five specialists. Start here.
   - **Code template experiment** — for custom routing logic later, create a
     Code Orchestrator Agent template (`run.py` using
     `sasram.agent.OrchestratorClient.get_langgraph_agent()`); the
     orchestrator-agent README includes a working sample.
3. Evaluate → champion → start. Test with a multi-step request, e.g.
   *"Profile PROCUREMENT_TRANSACTIONS, then build a model that predicts
   flagged transactions, then chart the top red flags."* — you should see the
   orchestrator delegate to Steward → Model Builder → Reporting in turn.

## Step 5 — Point the UI at it

In the NCGR RAM UI (`Finance_RAM_UI`), the orchestrator appears in the agent
dropdown like any other agent. Users get one assistant; RAM handles the
routing behind the scenes.

## Keep the monolith too

Keep the single full co-pilot (`sas-viya-copilot`, all tool groups) deployed
alongside for A/B comparison in the bootcamp — same image, just no
`TOOL_GROUPS` variable. See [`performance.md`](performance.md) for what to
measure when comparing the two patterns.
