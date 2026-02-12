---
description: Travel intelligence research and flight planning using TravelIntel MCP tools
---

You have access to TravelIntel MCP tools for comprehensive travel research.

## Tool Selection Guide

**Price search**: Use `travelintel_search_flights` — returns prices + schedule enrichment in one call. Do NOT also call `travelintel_search_schedules` since enrichment is already included.

**Schedule lookup**: Use `travelintel_search_schedules` — AeroDataBox primary (no date limit), FlightAware fallback (2-day limit).

**Deep route analysis**: Use `travelintel_investigate_route` — combines schedules + pricing + reliability + weather + knowledge base. Use for complex queries like "what are my options from CEB to Japan?"

**Route discovery**: Use `travelintel_discover_routes` — find all routes from an origin to a destination country/city/airport.

## Time Zone Rules

- FlightAware/AeroDataBox times via `search_schedules`: Real UTC. Convert to local (e.g. +8 for PHT, +9 for JST).
- SerpAPI times via `search_flights`: Times have Z suffix but are LOCAL times, NOT UTC. Never add timezone offset.
- Always cross-check critical flight times with `search_schedules` before presenting to user.

## Connection Minimums

- Domestic: 2 hours minimum
- International: 3 hours minimum

## Philippine Airport Reference

| Code | Airport | Key Routes |
|------|---------|------------|
| MNL | Manila NAIA | Hub for everything |
| CEB | Mactan-Cebu | International: SIN, domestic hub |
| TAC | Tacloban | MNL only (6/day) + CEB (2/day) |
| CYP | Calbayog | MNL only, zero Friday flights |

## Workflow

1. `travelintel_search_flights` for pricing
2. `travelintel_investigate_route` for deep analysis if needed
3. `travelintel_query_knowledge` to check what we already know about a route
4. Present options with prices, times, and connection feasibility
