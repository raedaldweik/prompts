# Platform Guide Agent — Setup

Answers "how do I…?" and walks users through SAS Viya. **Pure RAG — no MCP
tool server.** The collection *is* the agent, so invest there.

## 1. Collection (RAG)

Create `viya-platform-docs` and upload:
- [`collection/viya-getting-started-faq.md`](collection/viya-getting-started-faq.md)
- From [`../docs/rag-documents.md`](../docs/rag-documents.md): the
  *Introduction to SAS Viya Platform*, *SAS Viya Platform Overview*, and
  *SAS Studio User's Guide* PDFs — plus the guides for whichever products
  your users ask about most (Visual Analytics, Model Studio).

## 2. Agent

1. Create agent `Platform Guide`, paste [`system_prompt.md`](system_prompt.md).
2. Experiment (Tools-based, **no tools selected**): attach only the
   collection. Top K: 6–8 (documentation questions benefit from more context).
3. Champion → start.

## 3. A2A Agent Card (required for the orchestrator)

- **Description:** Answers "how do I…?" questions about SAS Viya (SAS Studio,
  Model Studio, Visual Analytics, data management) with step-by-step guidance
  from SAS documentation.
- **Skill — Platform how-to:** Gives numbered click-path instructions for
  tasks in SAS Viya applications, citing the documentation.
- **Skill — Concept & error explanations:** Explains SAS Viya concepts
  (caslibs, compute vs. CAS, pipelines) and interprets SAS error messages.
- **Query examples:**
  - How do I create a flow in SAS Studio?
  - How do I import a CSV into CAS?
  - What is a caslib?
  - How do I build a pipeline in Model Studio?
  - What does this SAS error mean?

## 4. Test

- "How do I create a report in Visual Analytics?" (expect steps + citation)
- "What's the difference between WORK and a caslib?"
