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

**Skills**: Auto-activates for travel queries with timezone rules, tool selection guide, HTML itinerary generation, and Philippine airport reference.

**Commands**:
- `/search-flights CEB MNL 2026-02-14` — quick flight search
- `/investigate-route CEB to Japan` — deep multi-layer analysis
- `/trip-briefing` — pre-trip intelligence briefing
- `/triphtml` — generate self-contained HTML itinerary from waterfall plan (Leaflet maps, price charts, airport breakdowns)

## HTML Itinerary Generation

The plugin includes a full design system for generating self-contained HTML travel itineraries:

- **Interactive Leaflet maps** with curved flight paths and airport markers
- **Price comparison bar charts** for airport/routing comparison
- **Two templates**: Flight Comparison (hero + alternatives) and Multi-Modal Journey (vertical timeline)
- **iOS-native aesthetic** — works perfectly in Safari on iPhone
- **Single file, no build** — just HTML + embedded CSS/JS + Leaflet CDN

Use `/triphtml` to generate from a TravelIntel waterfall plan, or the skill auto-activates when you ask for a visual itinerary.

## Requirements

- [Bun](https://bun.sh) runtime
- TravelIntel MCP server at `~/travel/app/` with `.env.local` configured
- API keys: DATABASE_URL, API_MARKET_KEY (AeroDataBox), SERPAPI_KEY, FLIGHTAWARE_API_KEY

## Plugin Structure

```
travelintel/
├── .claude-plugin/plugin.json     # Plugin metadata
├── .mcp.json                       # MCP server config (stdio via Bun)
├── commands/
│   ├── search-flights.md           # /search-flights
│   ├── investigate-route.md        # /investigate-route
│   ├── trip-briefing.md            # /trip-briefing
│   └── triphtml.md                 # /triphtml (HTML itinerary generation)
└── skills/
    └── travel/
        ├── SKILL.md                # Travel intelligence skill
        └── references/
            └── itinerary-html-format.md  # HTML design system (30KB)
```
