# Policy & Compliance Assistant — Setup

A pure-RAG agent: no MCP tools needed. It answers questions strictly from the
policy and procurement-law documents you upload into its collection. Good
"hello world" agent for the bootcamp — collection + prompt, nothing else.

## 1. Create the collection

1. In RAM, create a collection named e.g. `ncgr-policies`.
2. Upload the starter documents from [`collection/`](collection):
   - `ncgr-procurement-policy-summary.md`
   - `ncgr-data-governance-policy.md`
   - `ncgr-code-of-conduct.md`
3. Add the official English PDFs from
   [`../docs/rag-documents.md`](../docs/rag-documents.md): the **Government
   Tenders and Procurement Law** and its **Executive Regulations** (Ministry
   of Finance translations).
4. Let RAM chunk and embed them. (Replace the starter docs with your real
   policy PDFs/Word docs when ready — the agent works the same way.)

## 2. Create the agent

1. Create an agent and attach the `ncgr-policies` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. In **Retrieval Settings**, use a context-grounded prompt that keeps the
   `{context}` placeholder, for example:
   > Use the given context to answer the question. If the answer is not in the
   > context, say you do not know. Context: {context}

## 3. Retrieval settings

- **Top K:** 5–8.
- No tools required.

## 4. Test in the NCGR RAM UI

Pick this agent in the UI and try:
- "When is a direct award allowed instead of a public tender?"
- "Walk me through the steps for a tender on Etimad."
- "Is accepting a gift from a supplier allowed?"
- "What are the rules for sharing procurement data with a third party?"

The agent should answer with a citation and say so when something isn't covered.
