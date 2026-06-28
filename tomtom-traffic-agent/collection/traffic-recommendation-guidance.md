> **Sample document for the bootcamp.** How the agent should turn traffic data into advice.

# Turning Traffic Data into Recommendations (Sample)

## Always answer three questions
1. **What's happening now?** Current condition / ETA / incident — from live tools.
2. **What's the best option?** The recommended route or departure time, and the trade-off.
3. **How confident?** Note that ETAs are estimates and conditions change.

## Choosing a route
- Compare alternatives on **live ETA first**, then distance, then simplicity.
- Call out the trade-off explicitly: "X is 5 km longer but 10 min faster right now."
- Mention any incident or closure that drives the recommendation.

## Departure timing
- If asked "when should I leave to arrive by T?", work backwards from the live ETA and add a peak-period buffer (see the local context document).
- Suggest a small range ("leave between 6:40 and 6:55") rather than a single minute.

## Presenting the answer
- Lead with the recommendation, then show the **map** (route + any incidents).
- Use markers for origin/destination and incident overlays where relevant.
- Keep numbers rounded and human ("~25 min", "about 32 km").

## Responsible advice
- Never suggest illegal or unsafe actions (e.g. ignoring closures).
- Encourage buffer time rather than rushing.
- If live data is unavailable for the area, say so plainly instead of guessing.
