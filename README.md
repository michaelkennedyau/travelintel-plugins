# TravelIntel Plugins

Claude Code / Cowork plugins for travel intelligence.

## Install

```bash
claude plugin marketplace add michaelkennedyau/travelintel-plugins
claude plugin install travelintel@travelintel-plugins
```

## What's Included

**58 MCP tools** covering:
- Flight search (SerpAPI + AeroDataBox enrichment)
- Airport schedules (AeroDataBox + FlightAware)
- Route discovery and investigation
- Flight reliability analysis
- Waterfall trip planning with decision points
- Hotel search, weather, visa/safety checks
- Auto-learning knowledge base
- Price alerts and monitoring
- Currency conversion, itinerary export

**Skills**: Auto-activates for travel queries with timezone rules, tool selection guide, and Philippine airport reference.

**Commands**:
- `/search-flights CEB MNL 2026-02-14` — quick flight search
- `/investigate-route CEB to Japan` — deep multi-layer analysis
- `/trip-briefing` — pre-trip intelligence briefing

## Requirements

- [Bun](https://bun.sh) runtime
- TravelIntel MCP server at `~/travel/app/` with `.env.local` configured
- API keys: DATABASE_URL, API_MARKET_KEY (AeroDataBox), SERPAPI_KEY, FLIGHTAWARE_API_KEY
