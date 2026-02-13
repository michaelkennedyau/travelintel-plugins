# Travel Itinerary HTML Format — Component Library & Style Guide

This reference defines the complete CSS variable system, component library, and layout rules
for generating self-contained travel itinerary HTML files. Read this when the user requests
an itinerary visual, waterfall, or HTML travel document.

## Table of Contents

1. [Document Shell](#document-shell)
2. [CSS Variable System](#css-variable-system)
3. [Icon System](#icon-system)
4. [Layout Templates](#layout-templates)
5. [Component Library](#component-library)
6. [JavaScript Patterns](#javascript-patterns)
7. [Responsive Rules](#responsive-rules)
8. [Content Rules](#content-rules)

---

## Document Shell

Every itinerary is a single self-contained `.html` file. No external CSS or JS beyond the
Google Fonts import. Structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[ORIGIN] → [DEST] | Travel Itinerary | [DATE]</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>/* all CSS here */</style>
</head>
<body>
<div class="ctr">
  <!-- all content -->
</div>
<script>/* all JS here */</script>
</body>
</html>
```

The `.ctr` container is `max-width:960px;margin:0 auto`.

---

## CSS Variable System

Declare all variables on `:root`. Never use hard-coded colours in component CSS —
always reference variables. This makes theming consistent and maintainable.

### Core Tokens

```css
:root {
  /* Page */
  --white: #fff;
  --page: #fff;              /* ALWAYS pure white — no radial gradients on body */
  --wash: #f8f9fb;           /* light grey fill for cards, backgrounds */
  --wash2: #f1f3f6;          /* borders, dividers */
  --wash3: #e8ebf0;          /* deeper grey (progress bar tracks) */

  /* Text scale (darkest to lightest) */
  --t1: #111827;             /* headings, primary text */
  --t2: #374151;             /* body text */
  --t3: #6b7280;             /* secondary labels */
  --t4: #9ca3af;             /* muted, captions */
  --t5: #d1d5db;             /* borders, faint lines */

  /* Semantic colours — each has base, light bg, border */
  --g: #059669;   --gl: #ecfdf5;  --gb: #a7f3d0;  --gd: #047857;  /* green: good/recommended */
  --a: #b45309;   --al: #fffbeb;  --ab: #fde68a;                    /* amber: warning/caution */
  --r: #dc2626;   --rl: #fef2f2;  --rb: #fecaca;                    /* red: high risk/alert */
  --b: #2563eb;   --bl: #eff6ff;  --bb: #bfdbfe;                    /* blue: info/neutral */

  /* Border radius */
  --rad: 16px;               /* large cards, hero */
  --rad-sm: 10px;            /* inner cards, chips */

  /* Shadow scale */
  --sh-sm: 0 1px 3px rgba(0,0,0,0.04), 0 1px 2px rgba(0,0,0,0.03);
  --sh-md: 0 4px 16px rgba(0,0,0,0.06), 0 1px 4px rgba(0,0,0,0.04);
  --sh-lg: 0 8px 32px rgba(0,0,0,0.08), 0 2px 8px rgba(0,0,0,0.04);
  --sh-hero: 0 12px 48px rgba(5,150,105,0.10), 0 4px 16px rgba(0,0,0,0.05);
}
```

### Airline Brand Colours

Add per itinerary. Common ones:

```css
--qf: #d6001c;    /* Qantas */
--sq: #b8860b;    /* Singapore Airlines */
--cx: #005e5d;    /* Cathay Pacific */
--pr: #1a237e;    /* Philippine Airlines */
--jl: #bc002d;    /* JAL */
--5j: #ffd700;    /* Cebu Pacific */
--nh: #003399;    /* ANA */
--va: #e10029;    /* Virgin Australia */
--jq: #ff6200;    /* Jetstar */
```

### Transport Mode Colours (multi-modal)

For non-flight itineraries add mode-specific colours:

```css
--sea: #0369a1;   /* ferry/sea */
--bus: #7c3aed;   /* bus/coach */
--rail: #0891b2;  /* train */
--road: #78716c;  /* taxi/drive */
```

### Reset & Base

```css
* { margin:0; padding:0; box-sizing:border-box }
body {
  font-family: 'Inter', system-ui, sans-serif;
  background: var(--page);
  color: var(--t2);
  line-height: 1.55;
  padding: 32px 20px;
  -webkit-font-smoothing: antialiased;
}
```

---

## Icon System

All icons are inline SVG with a shared `.ic` class. Never use icon fonts or external SVG files.

```css
svg.ic { width:16px; height:16px; fill:none; stroke:currentColor;
         stroke-width:2; stroke-linecap:round; stroke-linejoin:round;
         vertical-align:-2px; flex-shrink:0 }
svg.ic-sm { width:13px; height:13px }
svg.ic-lg { width:20px; height:20px }
svg.ic-xl { width:24px; height:24px }
```

Common icon paths (use viewBox="0 0 24 24"):

| Icon | Path |
|------|------|
| Plane | `<path d="M17.8 19.2L16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-.9.1-1.1.5l-.3.5c-.2.4-.1.9.3 1.1l6.2 3.5-3.8 3.8-2-.6c-.3-.1-.7 0-.9.2l-.4.4c-.2.3-.2.7.1.9l2.8 1.9 1.9 2.8c.2.3.6.4.9.1l.4-.4c.2-.2.3-.6.2-.9l-.6-2 3.8-3.8 3.5 6.2c.2.4.7.5 1.1.3l.5-.3c.4-.2.6-.6.5-1.1z"/>` |
| Clock | `<circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/>` |
| Calendar | `<rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/>` |
| Shield | `<path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>` |
| Globe | `<circle cx="12" cy="12" r="10"/><path d="M12 2a14.5 14.5 0 0 0 0 20 14.5 14.5 0 0 0 0-20M2 12h20"/>` |
| User | `<path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/>` |
| Ticket | `<path d="M15 5v2m0 4v2m0 4v2M5 5a2 2 0 0 0-2 2v3a2 2 0 1 1 0 4v3a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-3a2 2 0 1 1 0-4V7a2 2 0 0 0-2-2H5z"/>` |
| Warning | `<path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/>` |
| Check | `<polyline points="20 6 9 17 4 12"/>` |
| Chevron down | `<polyline points="6 9 12 15 18 9"/>` |
| Star | `<polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>` |
| Bag | `<rect x="3" y="7" width="18" height="13" rx="2"/><path d="M8 7V5a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/>` |
| Dollar | `<line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/>` |
| Doc | `<path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/>` |
| Anchor | `<circle cx="12" cy="5" r="3"/><line x1="12" y1="22" x2="12" y2="8"/><path d="M5 12H2a10 10 0 0 0 20 0h-3"/>` |
| Bus | `<rect x="3" y="5" width="18" height="13" rx="2"/><path d="M3 10h18M7 18v2M17 18v2"/>` |

---

## Layout Templates

### Template A: Flight Comparison (Hero + Alternatives)

Use when comparing flight routing options between two airports. One option is "recommended"
and gets the hero treatment; others are compact alternatives.

**Layout order:**
1. **Header** — route, date, pax
2. **Contextual callout** (optional) — existing fare, special conditions
3. **Hero itinerary** — green banner + vertical timeline + footer
4. **Route map** (optional) — SVG with bold primary route, faded alternatives
5. **Comparison strip** — 4-up compact metric cards
6. **Alternative options** — compact cards with mini-itineraries + expandable details
7. **Planning notes** — collapsible section
8. **Footer** — generation date, sources, disclaimer

### Template B: Multi-Modal Journey

Use when the route involves mixed transport (ferries, buses, trains, flights, taxis).
No hero/alternatives split — one journey shown as a long vertical timeline.

**Layout order:**
1. **Header** — origin to destination, journey context
2. **Vertical timeline** — all segments with mode-specific styling
3. **Efficiency analysis** (optional) — grid of cards explaining route logic
4. **Critical warnings** (optional) — tight connections, visa issues
5. **Planning notes** — collapsible section
6. **Footer**

---

## Component Library

### Header

```css
.hdr { display:flex; justify-content:space-between; align-items:flex-end;
       margin-bottom:32px; padding-bottom:20px; border-bottom:2px solid var(--wash2) }
.hdr h1 { font-size:26px; font-weight:900; letter-spacing:-0.8px; color:var(--t1);
           display:flex; align-items:center; gap:10px }
.hdr .sub { color:var(--t3); font-size:12px; margin-top:5px;
             display:flex; align-items:center; gap:6px }
.hdr-r { text-align:right }
.hdr-r .dt { display:inline-flex; align-items:center; gap:6px; font-size:14px;
              font-weight:700; color:var(--t1); background:var(--wash);
              padding:7px 16px; border-radius:8px; border:1px solid var(--wash2) }
.hdr-r .pax { margin-top:6px; font-size:11px; color:var(--t4);
               display:flex; align-items:center; gap:5px; justify-content:flex-end }
```

```html
<div class="hdr">
  <div>
    <h1><svg class="ic ic-xl" viewBox="0 0 24 24" style="color:var(--g)"><!-- plane --></svg> Origin → Destination</h1>
    <div class="sub"><svg class="ic ic-sm" viewBox="0 0 24 24"><!-- globe --></svg> ORI Airport → DST Airport · Passengers · Class</div>
  </div>
  <div class="hdr-r">
    <div class="dt"><svg class="ic ic-sm" viewBox="0 0 24 24"><!-- calendar --></svg> Day DD Mon YYYY</div>
    <div class="pax"><svg class="ic ic-sm" viewBox="0 0 24 24"><!-- user --></svg> N × Adult · One-way/Return</div>
  </div>
</div>
```

### Hero Itinerary (Template A only)

The recommended option gets a prominent green-bordered card with a gradient banner.

```css
.hero { position:relative; background:var(--white); border:2px solid var(--gb);
        border-radius:var(--rad); box-shadow:var(--sh-hero); overflow:hidden; margin-bottom:32px }
.hero-banner { background:linear-gradient(135deg,#059669 0%,#047857 50%,#065f46 100%);
               padding:18px 24px; display:flex; justify-content:space-between; align-items:center }
.hero-banner h2 { color:var(--white); font-size:18px; font-weight:800; letter-spacing:-0.3px;
                   display:flex; align-items:center; gap:10px }
.hero-banner .hb-sub { color:rgba(255,255,255,0.7); font-size:11px; margin-top:2px }
.hero-badges { display:flex; gap:6px }
.hb { display:inline-flex; align-items:center; gap:4px; font-size:10px; font-weight:700;
      padding:4px 10px; border-radius:20px; background:rgba(255,255,255,0.18);
      color:var(--white); backdrop-filter:blur(4px); border:1px solid rgba(255,255,255,0.2) }
```

### Vertical Timeline

The core layout for showing journey progression. Each step is a dot + rail + body.

```css
.timeline { padding:28px 24px 20px; position:relative }
.tl-step { display:flex; gap:20px; position:relative; padding-bottom:0 }
.tl-step:last-child .tl-line { display:none }
.tl-rail { display:flex; flex-direction:column; align-items:center; width:48px;
           flex-shrink:0; position:relative; z-index:2 }
.tl-dot { width:48px; height:48px; border-radius:50%; display:flex; align-items:center;
          justify-content:center; font-size:10px; font-weight:800; color:var(--white);
          box-shadow:0 3px 10px rgba(0,0,0,0.12); position:relative; z-index:2 }
.tl-line { width:3px; flex:1; min-height:24px; margin:4px 0; border-radius:2px }
.tl-body { flex:1; padding-bottom:24px }
```

**Dot variants** — style by content type:

```css
.tl-dot.dot-ap  { background:var(--t1); font-size:11px; font-weight:900; letter-spacing:0.5px }  /* airport */
.tl-dot.dot-qf  { background:linear-gradient(135deg,var(--qf),#ff4d4d) }  /* airline-branded */
.tl-dot.dot-sq  { background:linear-gradient(135deg,var(--sq),#d4a017) }
.tl-dot.dot-conn { background:var(--white); border:2px dashed var(--gb);
                   width:36px; height:36px; margin:6px }  /* connection point */
.tl-dot.dot-conn svg { color:var(--g) }
```

For multi-modal, add mode-specific dots:

```css
.tl-dot.dot-loc  { background:var(--t3) }        /* generic location */
.tl-dot.dot-port { background:var(--sea) }        /* sea port */
.tl-dot.dot-apt  { background:var(--t1) }         /* airport */
.tl-dot.dot-ski  { background:#0ea5e9 }           /* ski resort / destination */
```

**Line variants** — style by transport mode:

```css
.tl-line.ln-fly  { background:linear-gradient(180deg,var(--t5),var(--t4)) }
.tl-line.ln-wait { background:repeating-linear-gradient(180deg,var(--gb) 0,var(--gb) 4px,transparent 4px,transparent 8px) }
.tl-line.ln-sea  { background:repeating-linear-gradient(180deg,var(--sea) 0,var(--sea) 6px,transparent 6px,transparent 10px) }
.tl-line.ln-road { background:linear-gradient(180deg,var(--road),#a8a29e) }
.tl-line.ln-bus  { background:linear-gradient(180deg,var(--bus),#a78bfa) }
.tl-line.ln-rail { background:linear-gradient(180deg,var(--rail),#22d3ee) }
```

### Airport Row (within timeline)

```css
.ap-row { display:flex; align-items:center; gap:14px; padding:10px 0 }
.ap-time { font-size:28px; font-weight:900; color:var(--t1); letter-spacing:-1.2px;
           font-variant-numeric:tabular-nums; line-height:1 }
.ap-meta .ap-name { font-size:14px; font-weight:700; color:var(--t1) }
.ap-meta .ap-detail { font-size:11px; color:var(--t3); margin-top:1px;
                      display:flex; align-items:center; gap:4px }
```

### Flight Card (within timeline)

```css
.fl-card { background:var(--wash); border:1px solid var(--wash2);
           border-radius:var(--rad-sm); padding:14px 16px; margin:4px 0 }
.fl-top { display:flex; justify-content:space-between; align-items:center; margin-bottom:10px }
.fl-id { display:flex; align-items:center; gap:8px }
.fl-logo { width:28px; height:28px; border-radius:50%; display:flex; align-items:center;
           justify-content:center; color:var(--white); font-size:10px; font-weight:800;
           box-shadow:0 2px 6px rgba(0,0,0,0.12) }
.fl-num { font-size:15px; font-weight:800; color:var(--t1) }
.fl-ac { font-size:11px; color:var(--t3); font-weight:500 }
.fl-stats { display:flex; gap:16px }
.fl-stat { display:flex; align-items:center; gap:5px; font-size:12px;
           font-weight:600; color:var(--t2) }
.fl-stat svg { color:var(--t4) }
.fl-bar { height:6px; background:var(--wash3); border-radius:3px; overflow:hidden }
.fl-bar-fill { height:100%; border-radius:3px; transition:width .8s ease }
```

The `.fl-bar-fill` width is proportional to flight duration relative to the longest segment.
Style with airline gradient: `background:linear-gradient(90deg,var(--XX),#lighter)`.

### Segment Card (multi-modal variant)

For non-flight segments (ferry, bus, taxi). Same structure as flight card but with mode
context instead of aircraft type.

```css
.seg-card { background:var(--wash); border:1px solid var(--wash2);
            border-radius:var(--rad-sm); padding:14px 16px; margin:4px 0 }
.seg-top { display:flex; justify-content:space-between; align-items:center; margin-bottom:10px }
.seg-id { display:flex; align-items:center; gap:8px }
.seg-logo { width:28px; height:28px; border-radius:8px; display:flex; align-items:center;
            justify-content:center; color:var(--white); font-size:10px; font-weight:800;
            box-shadow:0 2px 6px rgba(0,0,0,0.12) }
.seg-name { font-size:15px; font-weight:800; color:var(--t1) }
.seg-type { font-size:11px; color:var(--t3); font-weight:500 }
.seg-stats { display:flex; gap:16px }
.seg-stat { display:flex; align-items:center; gap:5px; font-size:12px;
            font-weight:600; color:var(--t2) }
.seg-bar { height:6px; background:var(--wash3); border-radius:3px; overflow:hidden }
.seg-bar-fill { height:100%; border-radius:3px; transition:width .8s ease }
```

### Connection Card

```css
.conn-card { background:var(--gl); border:1px solid var(--gb); border-radius:var(--rad-sm);
             padding:12px 16px; margin:4px 0; display:flex; align-items:center; gap:12px }
.conn-card .cc-time { font-size:20px; font-weight:800; color:var(--gd); letter-spacing:-0.5px }
.conn-card .cc-detail { font-size:11.5px; color:var(--t2); line-height:1.5 }
.conn-card .cc-detail strong { color:var(--gd); font-weight:700 }
```

Use green (`.gl`/`.gb`) for comfortable connections. For tight connections, swap to
amber (`.al`/`.ab`) or red (`.rl`/`.rb`) with matching text colour.

### Contextual Callout Bar

For existing bookings, special fares, or critical pre-context:

```css
.fare-bar { background:var(--al); border:1px solid var(--ab); border-radius:var(--rad-sm);
            padding:12px 18px; margin-bottom:28px; display:flex; align-items:center; gap:12px }
.fare-bar svg { color:var(--a); flex-shrink:0 }
.fare-bar p { font-size:12px; color:var(--t2); line-height:1.5 }
.fare-bar strong { color:var(--a); font-weight:700 }
```

### Info Chips

Small pill-shaped metadata tags. Used after timeline steps and in alt-card details.

```css
.chips { display:flex; gap:6px; flex-wrap:wrap; margin-top:12px }
.chip { display:inline-flex; align-items:center; gap:4px; font-size:10px; font-weight:600;
        color:var(--t3); padding:4px 10px; border-radius:20px; background:var(--white);
        border:1px solid var(--wash2) }
.chip svg { color:var(--t4) }
```

### Comparison Strip (Template A)

Four compact metric cards showing duration comparison across options:

```css
.comp-strip { display:flex; gap:6px; margin-top:20px; margin-bottom:24px }
.cs-item { flex:1; background:var(--wash); border:1px solid var(--wash2); border-radius:8px;
           padding:10px; text-align:center; position:relative; transition:all .2s; cursor:pointer }
.cs-item:hover { box-shadow:var(--sh-sm); border-color:var(--t5) }
.cs-item.cs-best { background:var(--gl); border-color:var(--gb) }
.cs-lbl { font-size:9px; font-weight:800; text-transform:uppercase; letter-spacing:.8px; margin-bottom:4px }
.cs-time { font-size:16px; font-weight:900; color:var(--t1); letter-spacing:-0.5px }
.cs-bar { height:4px; background:var(--wash3); border-radius:2px; margin-top:6px; overflow:hidden }
.cs-bar-fill { height:100%; border-radius:2px }
```

The bar width is proportional: shortest option = shortest bar (e.g. 80%), longest = 100%.
Best option gets `.cs-best` class for green highlight. Each card is clickable — scrolls
to the hero (for A) or toggles the relevant alt card (for B/C/D).

### Alternative Option Cards (Template A)

Compact cards for non-recommended options. Always show a mini-itinerary at a glance;
details expand on click.

**Card wrapper:**

```css
.alt-card { background:var(--white); border:1px solid var(--wash2); border-radius:var(--rad-sm);
            margin-bottom:10px; overflow:hidden; transition:all .25s }
.alt-card:hover { box-shadow:var(--sh-md); border-color:var(--t5) }
.alt-card.open { box-shadow:var(--sh-md) }
.alt-card.open .alt-chevron { transform:rotate(180deg) }
```

**Top bar** (option label + total time + risk):

```css
.alt-top { padding:14px 18px; display:flex; justify-content:space-between; align-items:center; gap:10px }
.alt-top-l { display:flex; align-items:center; gap:10px }
.alt-lbl { font-size:10px; font-weight:800; padding:4px 8px; border-radius:6px;
           text-transform:uppercase; letter-spacing:.5px }
.alt-lbl.l-b { background:var(--al); color:var(--a); border:1px solid var(--ab) }
.alt-lbl.l-c { background:var(--rl); color:var(--r); border:1px solid var(--rb) }
.alt-lbl.l-d { background:rgba(0,94,93,.07); color:var(--cx); border:1px solid rgba(0,94,93,.2) }
.alt-title { font-size:14px; font-weight:700; color:var(--t1) }
.alt-top-r { display:flex; align-items:center; gap:10px }
.alt-total { font-size:14px; font-weight:800; color:var(--t2); display:flex; align-items:center; gap:4px }
.alt-risk { font-size:9px; font-weight:800; padding:3px 8px; border-radius:4px;
            text-transform:uppercase; letter-spacing:.5px; display:inline-flex; align-items:center; gap:3px }
.rk-l { background:var(--gl); color:var(--g); border:1px solid var(--gb) }
.rk-m { background:var(--al); color:var(--a); border:1px solid var(--ab) }
.rk-h { background:var(--rl); color:var(--r); border:1px solid var(--rb) }
```

**Mini-itinerary** (always visible — shows route at a glance):

```css
.alt-itin { padding:0 18px 14px; display:flex; align-items:center; gap:0; flex-wrap:wrap }
.ai-node { display:flex; flex-direction:column; align-items:center; min-width:64px }
.ai-time { font-size:16px; font-weight:800; color:var(--t1); letter-spacing:-0.5px;
           font-variant-numeric:tabular-nums }
.ai-apt { font-size:10px; font-weight:700; color:var(--t3); margin-top:1px }
.ai-date { font-size:9px; color:var(--t4); margin-top:0 }
.ai-leg { display:flex; flex-direction:column; align-items:center; flex:1;
          min-width:80px; padding:0 6px }
.ai-leg-bar { width:100%; height:3px; border-radius:2px; position:relative; margin:2px 0 }
.ai-leg-info { display:flex; align-items:center; gap:4px; font-size:10px;
               font-weight:600; color:var(--t3) }
.ai-leg-info .al-dot { width:14px; height:14px; border-radius:50%; display:flex;
                       align-items:center; justify-content:center; color:var(--white);
                       font-size:6px; font-weight:800 }
.ai-conn { display:flex; flex-direction:column; align-items:center; padding:0 8px; min-width:48px }
.ai-conn-time { font-size:10px; font-weight:700; color:var(--t3) }
.ai-conn-icon { width:20px; height:20px; border-radius:50%; border:1.5px dashed var(--t5);
                display:flex; align-items:center; justify-content:center; margin:2px 0 }
.ai-conn-icon svg { width:10px; height:10px; color:var(--t4) }
```

Pattern: `ai-node → ai-leg → ai-node [→ ai-conn → ai-node → ai-leg → ai-node ...]`

**Expandable details:**

```css
.alt-detail-toggle { display:flex; align-items:center; justify-content:center; gap:5px;
                     padding:8px 18px; border-top:1px solid var(--wash2); cursor:pointer;
                     font-size:10px; font-weight:600; color:var(--t4); transition:all .2s;
                     background:var(--wash) }
.alt-detail-toggle:hover { color:var(--t2); background:var(--wash2) }
.alt-chevron { color:var(--t4); transition:transform .3s }
.alt-body { display:none; border-top:1px solid var(--wash2); padding:14px 18px;
            background:var(--wash); animation:fadeIn .25s ease }
.alt-body.show { display:block }
@keyframes fadeIn { from{opacity:0;transform:translateY(-4px)} to{opacity:1;transform:translateY(0)} }

.ab-warn { display:flex; align-items:center; gap:6px; padding:8px 12px;
           background:var(--rl); border:1px solid var(--rb); border-radius:6px;
           font-size:11px; color:var(--r); font-weight:600 }
.ab-note { font-size:11px; color:var(--t3); margin-top:8px; line-height:1.5;
           display:flex; align-items:flex-start; gap:6px }
.ab-chips { display:flex; gap:5px; flex-wrap:wrap; margin-top:8px }
```

### Efficiency Analysis Grid

For explaining route logic (especially multi-modal). 2-column grid of explanation cards.

```css
.eff-grid { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-top:24px }
.eff-card { background:var(--white); border:1px solid var(--wash2); border-radius:var(--rad-sm);
            padding:16px; box-shadow:var(--sh-sm) }
.eff-card h4 { font-size:11px; font-weight:800; color:var(--g); margin-bottom:8px;
               text-transform:uppercase; letter-spacing:.8px; display:flex; align-items:center; gap:5px }
.eff-card p { font-size:12px; color:var(--t2); line-height:1.6 }
```

### Planning Section (Collapsible)

```css
.plan-wrap { margin-top:8px }
.plan-toggle { width:100%; background:var(--wash); border:1px solid var(--wash2);
               border-radius:var(--rad-sm); padding:13px 18px; cursor:pointer;
               display:flex; align-items:center; justify-content:space-between;
               font-size:13px; font-weight:700; color:var(--t1); transition:all .2s }
.plan-toggle:hover { background:var(--wash2); box-shadow:var(--sh-sm) }
.plan-toggle .pt-l { display:flex; align-items:center; gap:8px }
.plan-toggle .chevron { transition:transform .3s; color:var(--t4) }
.plan-toggle.open .chevron { transform:rotate(180deg) }
.plan-body { display:none; padding:14px 0 0; animation:fadeIn .3s ease }
.plan-body.show { display:block }
.pg { display:grid; grid-template-columns:1fr 1fr; gap:10px }
.pi { background:var(--white); border-radius:var(--rad-sm); padding:14px;
      border:1px solid var(--wash2); box-shadow:var(--sh-sm) }
.pi h4 { font-size:10px; font-weight:800; color:var(--a); margin-bottom:8px;
         text-transform:uppercase; letter-spacing:.8px; display:flex; align-items:center; gap:5px }
.pi ul { list-style:none; padding:0 }
.pi li { font-size:11.5px; color:var(--t2); padding:3px 0 3px 14px; position:relative; line-height:1.5 }
.pi li::before { content:''; position:absolute; left:0; top:9px;
                 width:5px; height:5px; border-radius:50%; background:var(--a) }
```

### Hero Footer

```css
.hero-ft { display:flex; justify-content:space-between; align-items:center;
           padding:14px 24px; background:var(--wash); border-top:1px solid var(--wash2) }
.hero-ft-l { display:flex; align-items:center; gap:16px }
.hero-ft .tot { font-weight:900; color:var(--t1); font-size:15px;
                display:flex; align-items:center; gap:6px }
.hero-ft .tot-sub { font-size:11px; color:var(--t3); font-weight:500 }
.act-btns { display:flex; gap:8px }
.act-btn { display:inline-flex; align-items:center; gap:6px; padding:8px 16px;
           border-radius:8px; font-size:11px; font-weight:700; text-decoration:none;
           border:none; cursor:pointer; transition:all .2s }
.act-btn-primary { background:var(--g); color:var(--white);
                   box-shadow:0 2px 8px rgba(5,150,105,0.3) }
.act-btn-primary:hover { background:var(--gd); transform:translateY(-1px);
                         box-shadow:0 4px 12px rgba(5,150,105,0.35) }
.act-btn-sec { background:var(--white); color:var(--t2);
               border:1px solid var(--wash2); box-shadow:var(--sh-sm) }
.act-btn-sec:hover { border-color:var(--t5); box-shadow:var(--sh-md); transform:translateY(-1px) }
```

### SVG Route Map (Template A, optional)

A stylised SVG showing route geography. Primary route is bold with animation;
alternatives are thin and faded.

```css
.map-section { background:var(--wash); border:1px solid var(--wash2);
               border-radius:var(--rad); padding:16px; margin-bottom:28px; position:relative }
.map-section svg { width:100%; height:auto; display:block }
.map-label { position:absolute; top:12px; left:16px; font-size:9px; font-weight:700;
             text-transform:uppercase; letter-spacing:1.5px; color:var(--t4) }
```

Map SVG conventions:
- viewBox `"0 0 800 300"` with `<rect>` fill matching `var(--wash)`
- Land masses: `fill="var(--wash3)" stroke="var(--t5)" stroke-width="0.5"`
- Primary route: `stroke-width="3"`, `stroke-dasharray="8,5"`, animated `stroke-dashoffset`
- Alt routes: `stroke-width="1"`, `stroke-dasharray="4,4"`, `opacity="0.25"`
- Airport dots: `<circle>` with `stroke="var(--white)" stroke-width="2.5"`, text labels
- Origin/dest dots: `r="7"`, hub dots: `r="6"`, minor dots: `r="4"`

### Tooltip

```css
[data-tip] { position:relative; cursor:help }
[data-tip]:hover::after { content:attr(data-tip); position:absolute;
  bottom:calc(100% + 6px); left:50%; transform:translateX(-50%);
  background:var(--t1); color:var(--white); padding:5px 10px; border-radius:6px;
  font-size:10px; font-weight:500; white-space:nowrap; z-index:50;
  pointer-events:none; animation:fadeIn .15s ease }
```

### Footer

```css
.ftr { margin-top:32px; padding-top:14px; border-top:1px solid var(--wash2);
       text-align:center; font-size:10px; color:var(--t4); line-height:1.6 }
```

Always include: generation date, "Schedules indicative — verify with carriers", "All times local",
and list data sources.

---

## JavaScript Patterns

Keep JS minimal. Three standard functions for Template A:

```javascript
function toggleAlt(id) {
  const card = document.getElementById('alt' + id.toUpperCase());
  const body = document.getElementById('altBody' + id.toUpperCase());
  if (!card || !body) return;
  const isOpen = card.classList.contains('open');
  document.querySelectorAll('.alt-card').forEach(c => { c.classList.remove('open'); });
  document.querySelectorAll('.alt-body').forEach(b => { b.classList.remove('show'); });
  if (!isOpen) { card.classList.add('open'); body.classList.add('show');
    body.scrollIntoView({ behavior:'smooth', block:'nearest' }); }
}

function togglePlan() {
  const t = document.getElementById('planToggle'), b = document.getElementById('planBody');
  t.classList.toggle('open'); b.classList.toggle('show');
}

function scrollToHero() {
  document.querySelector('.hero').scrollIntoView({ behavior:'smooth', block:'start' });
}
```

Template B typically only needs `togglePlan()`.

---

## Responsive Rules

Single breakpoint at 768px:

```css
@media (max-width:768px) {
  body { padding:16px 12px }
  .hdr { flex-direction:column; gap:10px; align-items:flex-start }
  .hdr-r { text-align:left }
  .ap-time { font-size:22px }
  .tl-rail { width:36px }
  .tl-dot { width:36px; height:36px; font-size:8px }
  .tl-dot.dot-conn { width:28px; height:28px; margin:4px }
  .pg { grid-template-columns:1fr }
  .eff-grid { grid-template-columns:1fr }
  .comp-strip { flex-wrap:wrap }
  .cs-item { min-width:calc(50% - 4px) }
  .hero-banner { flex-direction:column; align-items:flex-start; gap:8px }
  .hero-ft { flex-direction:column; align-items:flex-start; gap:10px }
  .fl-top { flex-direction:column; align-items:flex-start; gap:6px }
}
```

---

## Content Rules

1. **All times local** — never display UTC. Apply timezone conversion from schedule data.
2. **Tabular numerics** — use `font-variant-numeric:tabular-nums` on all time displays.
3. **Connection risk colouring** — green ≥3h international / ≥2h domestic; amber 2–3h / 1.5–2h; red <2h / <1.5h.
4. **Bar proportions** — flight/segment bars are proportional to duration. Longest segment = 100% width.
5. **Airline logos** — use 2-letter IATA code in a coloured circle, never external image files.
6. **Date context** — show day-of-week + date on each timeline step. Flag "+1 day" arrivals.
7. **Source attribution** — always credit data sources in footer.
8. **No radial gradients on body** — ever. Pure `#ffffff` background.
9. **Risk labels** — Low/Med/High with green/amber/red semantic colours.
10. **Separate tickets warning** — if segments are on different PNRs, flag baggage recheck and no airline protection.
