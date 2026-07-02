# Why It Was Slow — and Every Lever to Make It Fast

A single agent turn stacks four latencies:

```
user → RAM (LLM plans) → MCP tool call → SAS Viya work → back through RAM (LLM writes answer)
```

Measured cold, the historic worst offender was **#3**: every SAS tool call
created a brand-new Viya compute session (a compute server pod spinning up —
routinely **15–45 seconds**) and deleted it afterwards. Everything else is
seconds. Fixing that is worth more than all other tuning combined.

## 1. Server-side fixes (already in `sas-mcp-server` / `SAS_Use_Case`)

On by default after the NCGR-bootcamp update — no configuration needed:

| Fix | What it saves |
|---|---|
| **Compute session reuse** (`COMPUTE_SESSION_REUSE=true`) | The 15–45s session start-up on every `execute_sas_code` / `query_table` / `generate_synthetic_data` call after the first. Dead sessions are detected and retried on a fresh one. |
| **Warm CAS connection** (use-case server) | The extra seconds `cas; caslib _all_ assign;` costs per query — now connected once per pooled session. |
| **Pooled HTTP connections** (`HTTP_CLIENT_POOL=true`) | A TCP+TLS handshake to Viya per tool call. |
| **Cached compute-context lookup** | One REST round trip per call. |
| **Fast-then-backoff job polling** (`JOB_POLL_INITIAL=0.25`) | ~1s average wait per short job (old fixed 2s poll). |
| **Output capping** (`MAX_SAS_OUTPUT_CHARS`) | Huge SAS logs entering the LLM context — smaller payloads mean faster LLM turns. |

Expected effect: a second `execute_sas_code` call drops from ~20–50s to
~1–3s + the actual SAS run time.

> Trade-off to know: a reused session keeps `WORK` datasets and macro
> variables between calls, like a real SAS session. Sessions are pooled per
> identity, never shared across users.

## 2. Fewer tools per agent (`TOOL_GROUPS`)

The LLM re-reads every attached tool schema on every turn, and more tools
means more wrong-tool detours. Slicing the 28-tool server into 6–10-tool
specialists (see [`orchestration.md`](orchestration.md)) cuts prompt size and
tool-choice errors — this is the main speed win of the orchestrator pattern.

## 3. RAM-side settings

- **Agentic retrieval** (agent experiment → Collections tab): the agent
  retrieves only from relevant collections instead of searching all of them —
  RAM's docs call out reduced processing time and cost. Requires each
  collection to have a champion config + description.
- **Top K**: 5–8. Every retrieved chunk is prompt tokens the LLM must read.
- **Experiment resources**: give agent experiments enough CPU/RAM (the
  experiment container runs the agent loop; starved containers are slow).
  1 CPU / 1Gi is fine for the tool servers; give the orchestrator and busy
  specialists 2 CPU if available.
- **LLM choice**: the planner LLM dominates turn time. A fast model for the
  specialists + a stronger model for the orchestrator is a good split if your
  RAM deployment offers several.

## 4. What the orchestrator does and doesn't buy you

- **Buys:** smaller tool surface per turn (faster planning, fewer errors),
  focused RAG per specialist (smaller Top-K contexts), and the ability to keep
  each specialist's conversation short. RAM decomposes multi-part requests
  and delegates tasks to the right agents.
- **Doesn't buy:** the orchestrator adds one extra LLM hop (route + summarize),
  so a *single trivial* question can be marginally slower than asking a lean
  specialist directly. The monolith co-pilot with all 28 tools is the slowest
  option for everything except one-off simple asks.

## 5. How to benchmark in the bootcamp

Ask the same three questions to (a) the monolith co-pilot and (b) the
orchestrator, and time them from the UI's trace panel (it shows every tool /
LLM / retrieval call with timings):

1. "List the tables in the Public caslib." *(pure tool call — measures server fix)*
2. "Profile PROCUREMENT_TRANSACTIONS and chart transactions by region." *(tool + chart)*
3. "Build a model to predict flagged transactions and report the champion." *(long AutoML — dominated by SAS run time, not the agent)*

Run each twice — the second run shows the warm-session effect.
