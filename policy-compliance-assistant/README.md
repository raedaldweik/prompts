# Policy & Compliance Assistant — Setup

A pure-RAG agent: no MCP tools needed. It answers questions strictly from the
policy documents you upload into its collection.

## 1. Create the collection
1. In RAM, create a collection named e.g. `rta-policies`.
2. Upload the starter documents from [`collection/`](collection):
   - `rta-data-governance-policy.md`
   - `rta-procurement-procedure.md`
   - `rta-code-of-conduct.md`
3. Let RAM chunk and embed them. (Replace these with your real policy PDFs/Word
   docs when ready — the agent works the same way.)

## 2. Create the agent
1. Create an agent and attach the `rta-policies` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. In **Retrieval Settings**, use a context-grounded prompt that keeps the
   `{context}` placeholder, for example:
   > Use the given context to answer the question. If the answer is not in the
   > context, say you do not know. Context: {context}

## 3. Retrieval settings
- **Top K:** 5–8.
- No tools required.

## 4. Test in Roads_RAM_UI
Pick this agent in the UI and try:
- "What are the rules for sharing data with a third party?"
- "Walk me through the procurement steps for a purchase over the single-source threshold."
- "Is accepting a gift from a supplier allowed?"

The agent should answer with a citation and say so when something isn't covered.
