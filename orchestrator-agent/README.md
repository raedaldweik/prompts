# Orchestrator Agent — Setup

The front door of Pattern B: one RAM **orchestrator agent** that routes every
request to the right specialist (Data Steward, Data Engineering, Model
Builder, Insights & Reporting, Platform Guide). Full architecture and
step-by-step build order: [`../docs/orchestration.md`](../docs/orchestration.md).

> Build the **five specialists first** (each with a configured A2A agent
> card) — an orchestrator experiment can only select agents whose A2A card is
> configured.

## 1. Create the orchestrator

1. **Agents → Create an Agent Orchestrator**.
2. Name: `NCGR SAS Viya Co-Pilot`.
3. Description: *One assistant for the full SAS Viya analytics lifecycle —
   understands the request and routes it to the right specialist agent.*
4. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.

## 2. Add an experiment (start no-code)

1. On the agent, add an experiment → **Basic no-code experiment**.
2. LLMs tab: pick your strongest planner LLM (routing quality depends on it).
3. **Agents tab:** select the five specialists.
4. Create → evaluate → select as **champion** → **start** the agent.

## 3. (Optional) Code-template orchestrator

For custom routing logic, create a **Code Orchestrator Agent** template in
Code Templates and use it in a "Code template experiment". Minimal working
`run.py` (from the RAM User's Guide):

```python
from langchain_core.messages import HumanMessage, SystemMessage
from sasram.agent import OrchestratorClient

async def exec(text: str, ram_client: OrchestratorClient) -> str:
    agent = await ram_client.get_langgraph_agent()
    session_history = ram_client.get_session_history_as_langchain()
    messages = (
        [SystemMessage("Use your sub-agents to inform your responses.")]
        + session_history
        + [HumanMessage(content=text)]
    )
    response = await agent.ainvoke({"messages": messages})
    return response["messages"][-1].content
```

## 4. Test (from the NCGR RAM UI)

- "What procurement data do we have and is it clean?" → *Data Steward*
- "Create a synthetic procurement dataset of 5,000 transactions." → *Data Engineering*
- "Build a model that predicts flagged transactions." → *Model Builder*
- "Chart award value by supplier for the top 10 suppliers." → *Insights & Reporting*
- "How do I create a pipeline in Model Studio?" → *Platform Guide*
- Multi-step: "Profile PROCUREMENT_TRANSACTIONS, build a risk model on it,
  and give me a chart of the top drivers." → three delegations in sequence.

Watch the routing in the UI's trace panel — every delegation shows up as a
sub-query with its own tool calls.
