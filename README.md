# RTA Bootcamp — Agent Prompts & Collections

This repository contains everything you need to build the five bootcamp agents in
**SAS Retrieval Agent Manager (RAM)**. For each use case there is a folder with:

- **`system_prompt.md`** — a ready-to-paste system prompt (the agent's persona,
  rules, and tool guidance).
- **`collection/`** — small starter documents to upload into the agent's
  **collection** (the RAG / vector database). Keep them small; you can always add
  your own real documents later.
- **`README.md`** — a step-by-step setup guide: which collection to create, which
  MCP tool server to attach (if any), the environment variables to set, and the
  retrieval settings to use.

## The five agents

| Folder | Agent | MCP tool server | Repo |
|--------|-------|-----------------|------|
| [`policy-compliance-assistant`](policy-compliance-assistant) | Policy & Compliance Assistant | _none (pure RAG)_ | — |
| [`driver-risk-intelligence-agent`](driver-risk-intelligence-agent) | Driver Risk & Behavioural Intelligence | SAS Use-Case MCP | `SAS_Use_Case` |
| [`ai-data-science-agent`](ai-data-science-agent) | AI & Data Science Agent | SAS MCP (full Viya) | `sas-mcp-server` |
| [`tomtom-traffic-agent`](tomtom-traffic-agent) | TomTom Real-Time Traffic | TomTom Maps MCP | `tomtom-maps-mcp` |
| [`news-intelligence-agent`](news-intelligence-agent) | News & Intelligence | News & Intelligence MCP | `Web_Search` |

All five are designed to be tested from the **Roads_RAM_UI** (the custom UI),
not RAM's own chat window.

## How every agent fits together in RAM

Each RAM agent needs **two** things, plus optionally a tool server:

1. **A collection (RAG).** Create a collection, upload the documents from the
   use case's `collection/` folder (drag-and-drop), and let RAM chunk + embed
   them. This is the agent's grounded knowledge.
2. **A system prompt.** Paste the contents of `system_prompt.md` into the
   agent's instructions. Where a use case relies on retrieved context, the
   prompt already references it — also confirm the **Retrieval Settings** system
   prompt keeps the `{context}` placeholder so retrieved chunks are injected.
3. **(Optional) An MCP tool server.** For the agents that act on data or live
   APIs, attach the MCP server listed above (as a *Container MCP Server* or
   *Code MCP Server*) and set its environment variables.

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
- **Grounding:** the prompts instruct the agents to answer from retrieved
  context and tool output, to cite sources, and to say when they don't know —
  this is the single biggest lever on trustworthiness.
- **Keep collections focused.** A tight, relevant collection beats a huge,
  noisy one. Replace the starter docs with your real policies/data dictionaries
  when you're ready.
