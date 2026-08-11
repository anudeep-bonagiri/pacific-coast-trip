# Pacific Coast Road Trip 🌲🌊🌴

**Seattle → Los Angeles · Aug 8–15, 2026 · 8 days, 7 nights · 3 travelers · ~1,550 miles**

A shareable trip site for three first-timers driving the Pacific Coast — Mount Rainier, the Oregon coast, the Redwoods, Big Sur, a real water day in SoCal, and a full LA day ending at Griffith at sunset.

## 🔗 Live site

Once deployed, share the Vercel link. Pages:

| Page | What it is |
|---|---|
| `index.html` | Landing page — route overview, budget, water-safety, booking checklist |
| `map.html` | **Interactive Leaflet map** — 51 planned stops, real driving routes, and your own saved places |
| `itinerary.html` | Hour-by-hour day-by-day plan, costs, booking list |
| `food.html` | **Food along the way** — dietary-safe orders, public-menu prices, and cheap backups for four travelers |
| `anudeep-plan.html` | **Anudeep’s Master Plan** — full visual day-by-day plan with route overview, directions, photos, reservations, and cut-if-late rules |
| `filming.html` | **Filming &amp; Scenic Spots** — day-by-day shot guide with best light, access notes, drone rules, and five hero shots |

## 📂 Source files

| File | What it is |
|---|---|
| `trip_data.json` | Machine-readable trip data: 8 days, 51 stops with coords, constraints, budget. **Source of truth.** |
| `roadtrip_itinerary.md` | The itinerary in Markdown |
| `CLAUDE_CODE_BRIEF.md` | Full project brief / handoff — locked-in facts, constraints, next tasks |

## ✏️ Editing the trip

The map is a **single self-contained HTML file** — no build step. Two arrays near the top of the `<script>` in `map.html` drive the planned itinerary: `DAYS` (8 entries) and `STOPS` (51 entries). Edit those to change the shared trip plan.

### Adding your own places

Open `map.html` and use **Add your own place** in the sidebar:

1. Search for a location and choose the correct result.
2. Assign it to a trip day and add timing/details.
3. Select **Save place**. The new pin and saved-place entry appear immediately.

Custom places are stored in the browser under `pacific-coast-trip.custom-places.v1` using `localStorage`. They survive refreshes and later visits at the same site URL in the same browser/profile. They intentionally do not edit `STOPS`, change route lines, or enter Git history. Clearing site data, using a private window, changing domains, or switching devices/browsers will not carry them over.

Location search uses the public [OpenStreetMap Nominatim service](https://nominatim.org/release-docs/latest/api/Search/) and is limited to U.S. results. No API key or build configuration is required for this small shared trip site. Search is submit-only (not autocomplete), enforces Nominatim's one-request-per-second maximum, displays OpenStreetMap attribution, and needs an internet connection. Review the [public service usage policy](https://operations.osmfoundation.org/policies/nominatim/) before changing the integration. If the site grows beyond light personal use, configure a hosted geocoder that permits the expected request volume and replace the search URL in `map.html`; follow the provider's usage and attribution requirements.

For reliable browser storage during local development, serve the folder over HTTP instead of double-clicking the file directly:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/map.html`.

Verify after editing:
```bash
awk 'f&&/<\/script>/{f=0} f{print} /<script>$/{f=1}' map.html > /tmp/app.js && node --check /tmp/app.js
```

## ⚠️ Keep these in any deliverable

- **Water safety** — the northern half is not swimmable (sneaker waves, 52–59°F). Swimming only in SoCal on Day 7.
- **Cash-only** warnings — Chandelier Drive-Thru Tree $15, Pfeiffer Beach $12. *(Fern Canyon was dropped when the route changed on Aug 11.)*
- Prices/conditions verified **mid-July 2026** — reverify anything time-sensitive before the trip.

*Built with Claude Code.*
