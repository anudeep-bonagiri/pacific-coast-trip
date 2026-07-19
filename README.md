# Pacific Coast Road Trip 🌲🌊🌴

**Seattle → Los Angeles · Aug 8–13, 2026 · 6 days, 5 nights · 3 travelers · ~1,200 miles**

A shareable trip site for three first-timers driving the Pacific Coast — Oregon coast, the Redwoods, Big Sur, and a real water day in SoCal.

## 🔗 Live site

Once deployed, share the Vercel link. Pages:

| Page | What it is |
|---|---|
| `index.html` | Landing page — route overview, budget, water-safety, booking checklist |
| `map.html` | **Interactive Leaflet map** — 43 stops with real driving routes, tap any pin |
| `itinerary.html` | Hour-by-hour day-by-day plan, costs, booking list |

## 📂 Source files

| File | What it is |
|---|---|
| `trip_data.json` | Machine-readable trip data: 6 days, 43 stops with coords, constraints, budget. **Source of truth.** |
| `roadtrip_itinerary.md` | The itinerary in Markdown |
| `CLAUDE_CODE_BRIEF.md` | Full project brief / handoff — locked-in facts, constraints, next tasks |

## ✏️ Editing the trip

The map is a **single self-contained HTML file** — no build step. Two arrays near the top of the `<script>` in `map.html` drive everything: `DAYS` (6 entries) and `STOPS` (43 entries). Edit those to change the trip.

Verify after editing:
```bash
awk 'f&&/<\/script>/{f=0} f{print} /<script>$/{f=1}' map.html > /tmp/app.js && node --check /tmp/app.js
```

## ⚠️ Keep these in any deliverable

- **Water safety** — the northern half is not swimmable (sneaker waves, 52–59°F). Swimming only in SoCal on Day 6.
- **Cash-only** warnings — Fern Canyon $12, Pfeiffer Beach $12.
- Prices/conditions verified **mid-July 2026** — reverify anything time-sensitive before the trip.

*Built with Claude Code.*
