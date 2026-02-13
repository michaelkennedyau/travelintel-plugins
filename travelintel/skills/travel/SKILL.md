---
description: Travel intelligence research and flight planning using TravelIntel MCP tools. Use this skill whenever the user asks about flights, routes, travel itineraries, trip planning, layover analysis, or wants a visual travel document. Also triggers for multi-modal journeys involving ferries, buses, trains, and mixed transport. Always use this skill when the user mentions travel, even if they don't explicitly ask for flight data.
---

You have access to TravelIntel MCP tools for comprehensive travel research.

## Tool Selection Guide

**Price search**: Use `travelintel_search_flights` — returns prices + schedule enrichment in one call. Do NOT also call `travelintel_search_schedules` since enrichment is already included.

**Schedule lookup**: Use `travelintel_search_schedules` — AeroDataBox primary (no date limit), FlightAware fallback (2-day limit).

**Deep route analysis**: Use `travelintel_investigate_route` — combines schedules + pricing + reliability + weather + knowledge base.

**Route discovery**: Use `travelintel_discover_routes` — find all routes from an origin to a destination country/city/airport.

## Time Zone Rules

- FlightAware/AeroDataBox times via `search_schedules`: Real UTC. Convert to local (e.g. +8 for PHT, +9 for JST).
- SerpAPI times via `search_flights`: Times have Z suffix but are LOCAL times, NOT UTC. Never add timezone offset.

## Connection Minimums

- Domestic: 2 hours minimum
- International: 3 hours minimum

## Workflow

1. `travelintel_search_flights` for pricing
2. `travelintel_investigate_route` for deep analysis if needed
3. `travelintel_query_knowledge` to check what we already know about a route
4. Present options with prices, times, and connection feasibility

---

## Output Formats

When the user asks for a visual itinerary, travel waterfall, or HTML travel document,
produce a self-contained HTML file using the itinerary format system below.

### When to generate HTML itineraries

Generate an itinerary HTML file when the user:
- Asks for a "waterfall", "visual", "itinerary", or "HTML" for a route
- Wants to compare routing options visually
- Requests a travel document they can open in a browser
- Asks for something that "looks like an itinerary"

### Choosing a template

There are two layout templates. Pick based on the journey type:

**Template A — Flight Comparison (Hero + Alternatives)**
Use when comparing 2–4 flight routing options between two airports. The best option
gets a prominent "hero" itinerary with full vertical timeline; alternatives are shown
as compact cards with mini-itineraries that expand on click. Good for airport-to-airport
planning where there's a clear recommended option.

**Template B — Multi-Modal Journey**
Use when the route involves mixed transport modes — ferries, buses, trains, taxis,
and flights combined. Shows one continuous journey as a long vertical timeline with
mode-specific styling (different line patterns, dot colours, segment cards). Good for
complex A-to-B journeys where the "how do I actually get there" question has one
answer with many legs.

### Building an itinerary

Before writing HTML, read the full component library and style guide:

```
Read references/itinerary-html-format.md
```

That reference file contains the complete CSS variable system, every component's
HTML/CSS pattern, icon paths, JavaScript functions, responsive rules, and content
rules. Follow it precisely — the design system is battle-tested and produces
professional, consistent output.

### Key design rules (quick reference)

These are the non-negotiable rules. The reference file has full detail.

1. **Single self-contained file** — all CSS in `<style>`, all JS in `<script>`, no external dependencies except Google Fonts (Inter).
2. **Pure white background** — `background: #fff` on body. No radial gradients. Ever.
3. **CSS variables for everything** — colours, radii, shadows. Never hard-code.
4. **Inline SVG icons** — use the `.ic` class system. No icon fonts, no external SVGs.
5. **All times local** — convert from UTC where needed. Use `font-variant-numeric: tabular-nums`.
6. **Connection risk colouring** — green ≥3h intl / ≥2h domestic, amber borderline, red tight.
7. **Airline identity** — 2-letter IATA code in a coloured circle. Brand colours from CSS variables.
8. **Bar proportions** — flight/segment duration bars are proportional. Longest = 100%.
9. **Separate tickets** — flag baggage recheck and no airline protection if segments on different PNRs.
10. **Footer** — generation date, "Schedules indicative — verify with carriers", "All times local", data sources.
11. **Responsive** — single breakpoint at 768px. Stack layouts, shrink timeline dots, wrap grids.
12. **Max width** — `.ctr` container at `max-width: 960px; margin: 0 auto`.

### File naming

Save itinerary files as: `travel-itinerary-{origin}-{dest}.html` (IATA codes, lowercase).
For multi-leg journeys with no clear IATA pair, use descriptive slugs:
`travel-itinerary-biliran-niseko.html`.

### Template A layout order

1. Header (route, date, pax)
2. Contextual callout (existing fare, special conditions) — if applicable
3. Hero itinerary (green banner → vertical timeline → hero footer with action buttons)
4. Route map SVG — optional, adds geographic context
5. Comparison strip (4-up metric cards, clickable)
6. Alternatives section header
7. Alternative cards B/C/D (top bar → mini-itinerary → expandable details)
8. Planning notes (collapsible)
9. Footer

### Template B layout order

1. Header (origin → destination, journey context)
2. Vertical timeline (all segments, mode-specific line/dot styles)
3. Efficiency analysis grid — optional, explains why each link was chosen
4. Critical warnings — tight connections, visa issues
5. Planning notes (collapsible)
6. Footer
