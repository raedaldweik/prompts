# News & Intelligence Agent — Setup

This agent uses the **News & Intelligence MCP** (`Web_Search` repo), which is
Tavily-backed and supports an **approved-domains** allow-list. For NCGR it
monitors procurement developments, supplier/market news, regulation, and
economic signals.

## 1. Get a Tavily key

Create a free key at https://tavily.com.

## 2. Create the collection (RAG)

1. Create a collection named e.g. `news-intelligence-context`.
2. Upload the starter documents from [`collection/`](collection):
   - `monitoring-watchlist.md` — the topics NCGR cares about
   - `trusted-sources.md` — preferred sources / rationale for the allow-list
   - `briefing-format.md` — the house style for briefings
3. Tune the watchlist to your interests.

## 3. Attach the News & Intelligence MCP tool server

Host the `Web_Search` server (Container MCP Server) — or paste
`examples/ram_code_tool.py` into a **Code MCP Server**. Environment:

```bash
TAVILY_API_KEY=<your tavily key>

# Optional: restrict to approved domains (allow-list). Sub-domains match.
# Leave empty to search the open web.
APPROVED_DOMAINS=spa.gov.sa,mof.gov.sa,etimad.sa,argaam.com,arabnews.com,saudigazette.com.sa,reuters.com
BLOCKED_DOMAINS=
DOMAIN_ENFORCE=true

DEFAULT_TIME_RANGE=week
MAX_RESULTS=8
MCP_API_KEY=<set a key and give it to RAM>
```

Point RAM at the server's `/mcp` endpoint (send the `MCP_API_KEY` if set).

## 4. Create the agent

1. Attach the `news-intelligence-context` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the News & Intelligence MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder.

## 5. Retrieval settings

- **Top K:** 4–6.

## 6. Test in the NCGR RAM UI

- "What's new in Saudi government procurement this week?"
- "Brief me on Etimad platform announcements this month."
- "Monitor 'public procurement digitalization' across regulation, technology,
  and integrity angles."
