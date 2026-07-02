# NCGR Agentic AI Bootcamp — Agent Prompts & Collections

Everything you need to build the bootcamp agents in **SAS Retrieval Agent
Manager (RAM)** for the **National Center for Government Resources Systems
(NCGR)**, Saudi Arabia — the operator of the Etimad platform for government
procurement. For each agent there is a folder with:

- **`system_prompt.md`** — a ready-to-paste system prompt (the agent's persona,
  rules, and tool guidance).
- **`collection/`** — small starter documents to upload into the agent's
  **collection** (the RAG / vector database). Keep them small; replace them
  with your real documents when ready. `docs/rag-documents.md` lists the
  public PDFs (SAS documentation, Saudi procurement law, OECD/World Bank
  fraud guidance) to add to each collection.
- **`README.md`** — a step-by-step setup guide: which collection to create,
  which MCP tool server to attach (if any), the environment variables to set,
  the **A2A agent card** (needed for orchestrator routing), and test prompts.

## The two patterns

### Pattern A — one full co-pilot (simplest)

| Folder | Agent | MCP tool server | Repo |
|--------|-------|-----------------|------|
| [`sas-viya-copilot`](sas-viya-copilot) | SAS Viya Co-Pilot (everything in one agent) | SAS MCP (full Viya, ~28 tools) | `sas-mcp-server` |

One agent, all tools. Powerful but slower: the LLM reads ~28 tool schemas on
every turn and long multi-step tasks stack up in one context.

### Pattern B — orchestrated specialists (faster per step, parallel-friendly)

One **orchestrator agent** routes each request to the right specialist. Every
specialist runs the **same `sas-mcp-server` image**, deployed as a separate
RAM tool server with a different `TOOL_GROUPS` slice — plus its own focused
collection (RAG).

| Folder | Agent | `TOOL_GROUPS` | Own RAG collection |
|--------|-------|---------------|--------------------|
| [`orchestrator-agent`](orchestrator-agent) | Orchestrator — routes to the specialists | — | — |
| [`data-steward-agent`](data-steward-agent) | Data Steward — find, profile, quality-check | `data, insights, charts` | Information Catalog / data quality docs |
| [`data-engineering-agent`](data-engineering-agent) | Data Engineering — clean, join, shape, generate | `data, datamgmt, sas, synth, files, jobs` | SAS Studio flows / CAS docs |
| [`model-builder-agent`](model-builder-agent) | Model Builder — AutoML, register, score | `data, ml, score, jobs` | Model Studio / Model Manager docs |
| [`insights-reporting-agent`](insights-reporting-agent) | Insights & Reporting — visuals leaders can act on | `data, charts, insights, sas` | Visual Analytics docs |
| [`platform-guide-agent`](platform-guide-agent) | Platform Guide — "how do I…?" on SAS Viya | *(none — pure RAG)* | Viya platform / getting-started docs |

See [`docs/orchestration.md`](docs/orchestration.md) for the full build steps
(A2A cards, agent experiments, the no-code orchestrator) and
[`docs/performance.md`](docs/performance.md) for why this pattern is faster
and every other speed lever.

### Use-case agents (talk to a *finished* project)

| Folder | Agent | MCP tool server | Repo |
|--------|-------|-----------------|------|
| [`procurement-integrity-agent`](procurement-integrity-agent) | Procurement Integrity — the flagship NCGR use case: query the procurement data, explain red flags, score transactions in real time | SAS Use-Case MCP (scoped) | `SAS_Use_Case` |
| [`policy-compliance-assistant`](policy-compliance-assistant) | Policy & Compliance — answers from the procurement law/policy corpus | *(none — pure RAG)* | — |
| [`news-intelligence-agent`](news-intelligence-agent) | News & Intelligence — procurement/supplier/economy monitoring | News & Intelligence MCP | `Web_Search` |

The co-pilot (Pattern A/B) **builds** projects: generate synthetic procurement
data, clean it, model it, deploy it. The use-case agents then **talk to** the
finished project: the procurement-integrity agent is scoped to that data and
model, grounded in the legal corpus, and can score in real time.

All agents are designed to be tested from the **NCGR RAM UI**
(`Finance_RAM_UI` repo), not only RAM's own chat window.

## How every agent fits together in RAM

Each RAM agent needs **two** things, plus optionally a tool server:

1. **A collection (RAG).** Create a collection, upload the documents from the
   agent's `collection/` folder (drag-and-drop) plus the PDFs listed in
   `docs/rag-documents.md`, and let RAM chunk + embed them.
2. **A system prompt.** Paste the contents of `system_prompt.md` into the
   agent's instructions. Where an agent relies on retrieved context, the
   prompt already references it — also confirm the **Retrieval Settings**
   system prompt keeps the `{context}` placeholder so retrieved chunks are
   injected.
3. **(Optional) An MCP tool server.** For the agents that act on data or live
   APIs, attach the MCP server listed above (as a *Container MCP Server*) and
   set its environment variables.
4. **(Orchestration only) An A2A agent card.** The orchestrator routes by each
   agent's A2A card — copy the description, skills, and query examples from
   the agent's README onto its A2A Agent Card tab.

### A note on system prompts vs. retrieval `{context}`

RAM has two prompt surfaces:

- the **agent instructions / system prompt** (persona + behaviour + tool
  guidance) — this is what each `system_prompt.md` contains; and
- the **Retrieval Settings** prompt, which must contain `{context}` (e.g.
  *"Use the given context to answer the query… Context: {context}"*) so the
  retrieved chunks are passed to the model.

Keep both. The `system_prompt.md` shapes how the agent behaves; the retrieval
prompt is what physically injects the RAG chunks.

## Tips for good results

- **Top K (query result limit):** start at 5–8. Raise it if answers miss
  context; lower it if the agent gets distracted.
- **Enable agentic retrieval** on agents with more than one collection — the
  agent then retrieves only from the relevant collection, which cuts latency
  and cost (each collection needs a champion configuration and a description).
- **Grounding:** the prompts instruct the agents to answer from retrieved
  context and tool output, to cite sources, and to say when they don't know —
  this is the single biggest lever on trustworthiness.
- **Keep collections focused.** A tight, relevant collection beats a huge,
  noisy one. Replace the starter docs with your real policies/data
  dictionaries when you're ready.
