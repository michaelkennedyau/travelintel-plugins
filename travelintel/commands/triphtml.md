# /triphtml -- Generate Travel Itinerary HTML with Maps

Generate a beautiful, self-contained HTML itinerary page from a TravelIntel waterfall plan. Includes interactive Leaflet maps with curved flight paths, price comparison workings, and discarded route analysis.

## Quick Reference

| Command | Action |
|---------|--------|
| `/triphtml` | Generate from most recent trip/plan |
| `/triphtml [planId]` | Generate from specific waterfall plan |
| `/triphtml [planId] mobile` | Mobile-optimized (default) |
| `/triphtml [planId] web` | Desktop-optimized with wider layout |

## Initialization

When `/triphtml` is invoked:

1. **Get the waterfall plan data:**
   - If a `planId` argument is provided, use it
   - Otherwise, call `travelintel_list_trips` to find the most recent trip, then `travelintel_get_waterfall` with its plan ID
   - Parse the full waterfall: plan metadata, decision points, options (recommended vs discarded), legs

2. **Determine format:**
   - Parse second argument: `mobile` (default) or `web`
   - Mobile: max-width 100%, viewport locked, larger touch targets
   - Web: max-width 680px centered, larger map (480px), 3-column grids

3. **Identify the narrative:**
   - Which decision point is the main one (most options, or user-specified)?
   - What's the recommended option (first sortOrder, or user-locked)?
   - What options were discarded and why (from option `condition` field)?
   - What's the savings vs the most expensive alternative?

## HTML Generation Steps

### Step 1: Collect Geographic Data

Build an airport coordinate lookup from the leg data. Use this reference for common airports:

```javascript
var AIRPORT_COORDS = {
  // Japan
  NGO: [34.858, 136.813], KIX: [34.435, 135.244],
  NRT: [35.772, 140.393], HND: [35.549, 139.780],
  FUK: [33.586, 130.451], CTS: [42.775, 141.692],
  KOJ: [31.421, 130.720], OKA: [26.196, 127.646],
  ITM: [34.785, 135.438],
  // Southeast Asia
  MNL: [14.509, 121.020], CEB: [10.307, 123.979],
  SIN: [1.350, 103.994], BKK: [13.681, 100.747],
  HKG: [22.309, 113.915], SGN: [10.819, 106.652],
  // Australia
  BNE: [-27.384, 153.118], SYD: [-33.946, 151.177],
  MEL: [-37.673, 144.843], PER: [-31.940, 115.967],
  // Key cities (for train legs)
  Kyoto: [35.012, 135.768], Tokyo: [35.682, 139.769],
  Osaka: [34.694, 135.502], Nagoya: [35.181, 136.906],
  // Other common
  ICN: [37.463, 126.441], NBO: [37.460, 126.440],
  TPE: [25.080, 121.233], KUL: [2.746, 101.710],
  DOH: [25.261, 51.565], DXB: [25.253, 55.366],
  LHR: [51.470, -0.461], LAX: [33.943, -118.408],
  SFO: [37.619, -122.375], AKL: [-37.008, 174.792]
};
```

For any airport NOT in this list, use `travelintel_search_airports` to get coordinates, or estimate from the route context.

### Step 2: Generate the HTML Structure

Create a single self-contained HTML file with this structure:

```
1. HEADER
   - Trip name, date, passenger count
   - Gradient background (#0a84ff → #5856d6)

2. SAVINGS BANNER (if savings calculable)
   - Big A$ amount saved
   - "vs [most expensive alternative]"
   - Green gradient background

3. ROUTE MAP (Leaflet.js)
   - Interactive map with CARTO light basemap
   - Curved flight paths (quadratic bezier)
   - Recommended route: thick blue (#0a84ff) solid curves
   - Booked legs: thick green (#34c759) solid curves
   - Train/ground legs: orange (#ff9500) dashed lines
   - Discarded routes: thin gray (#c7c7cc) dashed curves
   - Airport markers with price labels (blue=winner, gray=discarded)
   - Destination markers (green)
   - Plane emoji at curve midpoints on recommended route
   - Legend below map
   - fitBounds to show all points with padding

4. ALREADY BOOKED section (if any legs have status="booked")
   - Green "Confirmed" badge
   - Flight details

5. RECOMMENDED JOURNEY flow
   - Horizontal flow: Place → mode → Place → mode → Place

6. HOW WE FOUND THE BEST ROUTE (the workings)
   - Title: "We searched N airports" or "We compared N options"
   - CSS bar chart showing price per person at each airport/option
     - Bars scaled relative to most expensive (100%)
     - Winner: blue gradient, labeled "BEST VALUE"
     - Mid-range: gray (#8e8e93)
     - Most expensive: red gradient
     - Each bar shows: airport code, city name, price, verdict

7. AIRPORT/OPTION BREAKDOWN
   - Grid of cards (2-col mobile, 3-col desktop)
   - Winner card: blue left border, blue background tint
   - Eliminated cards: red left border, strikethrough code
   - Each card: code, city, price, travel time, elimination reason

8. FLIGHT OPTIONS from recommended airport
   - Recommended option: blue border, "RECOMMENDED" badge
   - Other viable options: plain white cards
   - Conditional options (e.g. "only if going to Tokyo"): dimmed (opacity 0.7)
   - Each card: airline, flight number, price/pp, tags (bags, risk, total), leg timeline

9. KEY INSIGHT note
   - Yellow card summarizing the main finding
   - Bold the savings amount

10. FOOTER
    - Research date, times disclaimer, generation attribution
```

### Step 3: Map JavaScript

Use this curved path algorithm:

```javascript
function curve(from, to, offset) {
  var dLat = to[0] - from[0], dLng = to[1] - from[1];
  var dist = Math.sqrt(dLat * dLat + dLng * dLng);
  if (dist === 0) return [from, to];
  var midLat = (from[0] + to[0]) / 2 + offset * (-dLng / dist);
  var midLng = (from[1] + to[1]) / 2 + offset * (dLat / dist);
  var points = [];
  for (var t = 0; t <= 1; t += 0.015) {
    var u = 1 - t;
    points.push([
      u*u*from[0] + 2*u*t*midLat + t*t*to[0],
      u*u*from[1] + 2*u*t*midLng + t*t*to[1]
    ]);
  }
  return points;
}
```

**Offset guidelines:**
- Long routes (>30 degrees): offset 4-6
- Medium routes (10-30 degrees): offset 2-4
- Short routes (<10 degrees): offset 1-2
- Vary sign (+/-) for routes that overlap to prevent visual clutter

### Step 4: Styling Requirements

**Mobile-first design (iOS-native feel):**
- Font: -apple-system, BlinkMacSystemFont, SF Pro Display
- Background: #f2f2f7 (iOS grouped background)
- Cards: white, border-radius 16px, subtle shadow
- Section titles: 13px uppercase, #8e8e93
- Safe area padding for notch devices
- Map height: 380px mobile, 480px desktop
- scrollWheelZoom: false on map (prevents scroll hijacking)

**Color palette:**
- Primary blue: #0a84ff
- Success green: #34c759
- Warning orange: #ff9500
- Danger red: #ff3b30
- Gray text: #636366, #8e8e93
- Muted: #c7c7cc, #aeaeb2

**CDN dependencies (inline in HTML):**
```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```

Basemap: `https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png`

### Step 5: Web vs Mobile Differences

**Mobile (default):**
- Full-width layout
- 2-column airport grid (1-column on very narrow screens <420px)
- 380px map height
- Touch-friendly targets (44px minimum)
- viewport: width=device-width, maximum-scale=1.0

**Web:**
- max-width: 680px, centered
- 3-column airport grid
- 480px map height
- Hover effects on cards
- Add `user-scalable=yes` to viewport

### Step 6: Save and Report

1. Save to `~/travel/{trip-name-slugified}.html`
   - Slugify the trip name (lowercase, hyphens, no special chars)
   - If no trip name, use `trip-{date}.html`
2. Report to user:
   - File path
   - How to share (AirDrop, iMessage, email)
   - Offer to open in browser for preview

## Data Mapping from Waterfall

Map waterfall data to HTML elements:

| Waterfall Field | HTML Element |
|-----------------|-------------|
| `plan.name` | Header title |
| `plan.description` | Used for narrative context |
| `plan.notes` | Key insight note card |
| `point.trigger` | Section context |
| `option.label` | Option card title |
| `option.condition` | Elimination reason / option description |
| `option.costMin` | Price display and bar chart |
| `option.riskLevel` | Risk tag (low=green, medium=orange, high=red) |
| `option.isLocked` | If locked, mark as "SELECTED" instead of "RECOMMENDED" |
| `leg.mode` | Route line style (flight=curved, train=dashed, bus=dotted) |
| `leg.origin` / `leg.destination` | Map markers and coordinates |
| `leg.operator` | Airline/operator name |
| `leg.flightNumber` | Flight number display |
| `leg.scheduledDeparture/Arrival` | Time display |
| `leg.status` | "booked" → green confirmed badge |
| `leg.contingency` | Fallback info in leg detail |

## Error Handling

- **No trips found**: Tell user to create a trip first with TravelIntel
- **No decision points**: Generate a simpler page with just the route map and legs
- **Missing coordinates**: Use `travelintel_search_airports` or ask user
- **No price data**: Skip savings banner and bar chart, focus on route map

## Example Output

For a Japan→Brisbane trip, the generated HTML should include:
- An interactive map spanning Japan to Australia with curved blue/green flight paths
- Gray dashed lines showing 5 eliminated airport routes
- A bar chart comparing 6 airports by price
- Cards explaining why each airport was eliminated
- The recommended flight option highlighted in blue
- A yellow note card with the key savings insight

The file should open perfectly in Safari on iPhone with no external dependencies except the Leaflet CDN.
