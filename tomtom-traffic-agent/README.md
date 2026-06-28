# TomTom Real-Time Traffic Agent — Setup

This agent uses the **TomTom Maps MCP** (`tomtom-maps-mcp` repo) for live
traffic, routing, places, and map rendering. The Roads_RAM_UI renders the map
specifications the agent returns.

## 1. Get a TomTom API key
Create a key at https://developer.tomtom.com/. You'll use it in two places:
- the **MCP server** (so it can call TomTom APIs), and
- the **Roads_RAM_UI** build (`VITE_TOMTOM_API_KEY`) so the map tiles render.

## 2. Create the collection (RAG)
1. Create a collection named e.g. `dubai-traffic-context`.
2. Upload the starter documents from [`collection/`](collection):
   - `dubai-traffic-context.md` — key corridors, areas, peak times
   - `traffic-recommendation-guidance.md` — how to turn data into advice

## 3. Attach the TomTom MCP tool server
Host `tomtom-maps-mcp` (Container MCP Server). Minimum environment (see that
repo's `.env.example`):

```bash
TOMTOM_API_KEY=<your tomtom key>
PORT=3000
# MCP_BASE_URL=https://<your-host>   # if behind a public URL
```

Point RAM at the server's MCP endpoint.

## 4. Create the agent
1. Attach the `dubai-traffic-context` collection.
2. Paste [`system_prompt.md`](system_prompt.md) into the agent's instructions.
3. Attach the TomTom MCP tool server.
4. In **Retrieval Settings**, keep the `{context}` placeholder.

## 5. Retrieval settings
- **Top K:** 4–6.

## 6. Test in Roads_RAM_UI
- "What's the fastest route from Dubai Mall to Dubai Airport right now?" (expect a map + ETA)
- "Any incidents on Sheikh Zayed Road at the moment?"
- "When should I leave Deira to reach Abu Dhabi by 9am?"

> Make sure `VITE_TOMTOM_API_KEY` was set at **build time** of the UI, or the
> map tiles will be blank even though the agent returns a valid map spec.
