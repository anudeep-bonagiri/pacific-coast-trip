# Claude Code Handoff — Pacific Coast Road Trip

Paste this whole file (plus `trip_data.json`) into Claude Code. It contains everything needed to continue the project with no prior conversation context.

---

## 1. What this project is

Planning a **Seattle → Los Angeles Pacific Coast road trip** for **3 guys, ages 19–20, first time on the coast**, and building an **interactive map** deliverable.

**Trip:** Aug 8–13, 2026 · 6 days, 5 nights · ~1,200 miles · fly into SEA, out of LAX.

### Files in this project
| File | What it is |
|---|---|
| `pacific_coast_roadtrip_map.html` | **Main deliverable.** Self-contained interactive Leaflet map. Open in any browser. |
| `trip_data.json` | Machine-readable trip data: 6 days, 43 stops with coords, all constraints, budget. **Source of truth for the data.** |
| `roadtrip_itinerary.md` | Human-readable timed itinerary + costs + safety notes. |
| `CLAUDE_CODE_BRIEF.md` | This file. |

### How the map works (architecture)
Single self-contained HTML file. No build step, no framework.
- **Leaflet 1.9.4** + CARTO Voyager tiles, both from `cdnjs.cloudflare.com`.
- Two data arrays near the top of the `<script>`: `DAYS` (6 entries) and `STOPS` (43 entries). **Edit these to change the trip** — everything else derives from them.
- Each stop has a `kind` that controls its marker:
  - `sight` → numbered circle (auto-numbered in drive order)
  - `gem` → ★ (hidden gems / best viewpoints)
  - `water` → 🌊 (water activities)
  - `bonus` → dashed "+" (optional squeeze-ins)
  - `sleep` → 🛏 (lodging)
  - `air` → ✈ (airports)
- **Route lines follow real roads**: `routeByRoad()` fetches driving geometry per day from the public OSRM server (`router.project-osrm.org`), then replaces the straight-line fallback. If the fetch fails (offline/rate-limited), straight lines remain — always renders.
- `bonus` stops are **excluded** from the route line and the day-navigation links (via `routeStops`) so optional detours don't distort the main route.
- Every popup has Google Maps / Directions / Info links; sleep stops get a "Hotels" search link. Each day header has a "Navigate this day in Google Maps" button.
- Collapsible info boxes at top: water safety, flights, Car Week, Big Sur road control, weather, rental notes.

**Verify after editing:** extract and syntax-check the script —
```bash
awk 'f&&/<\/script>/{f=0} f{print} /<script>$/{f=1}' pacific_coast_roadtrip_map.html > /tmp/app.js && node --check /tmp/app.js
```
Also sanity-check that latitudes within each day run roughly north→south (the trip is one continuous southbound drive). Exceptions: Day 1 (airport is south of downtown Seattle — they train north and drive back south) and Day 6 (the LA coast runs east–west, so check longitude instead).

---

## 2. Locked-in facts (do not re-litigate)

**Travelers:** 3 people, 19–20 years old. **All under 21** — do not suggest bars/breweries as drinking activities (fine as restaurants). Budget-conscious: cheap motels, one room split 3 ways.

**Flights**
- **Out · Sat Aug 8:** SAT 7:30 AM → LAX (1h12 layover) → **lands SEA 12:27 PM**. Delta, $124.
- **Back · Thu Aug 13:** ⚠️ **NEEDS REBOOKING.** Original was LAX 6:59 PM on Aug 12 ($135).
  - **Verify fare class first — Delta Basic Economy generally cannot be changed at all.**
  - Book the **latest departure possible (8–9 PM)** — that's the only way Griffith Observatory / Hollywood Sign fits on Day 6.

**Car:** Hertz, **$663**. Pickup **SEA 5:00 PM Aug 8** (this is why Day 1 morning is car-less — they use Link light rail downtown). Drop at LAX. Under-25 surcharge applies; **all three must be on the contract** to rotate drivers. Gas budgeted **$210–230**.

**Decisions already made (with reasons):**
- **Olympic Peninsula skipped** — too far north, added ~200 miles for a trip this short.
- **6th day added (return Aug 13)** — this **eliminated the overnight-split drive**. They now sleep in a bed every night, and hit the Golden Gate in daylight instead of 11 PM. Do not reintroduce night driving.
- **Sleep Salinas, not Monterey** — Car Week.
- **Sleep Garberville, not Eureka** — avoids a 45-min backtrack to Avenue of the Giants.
- **Sleep Pismo night 5** — further south shortens the final morning to Malibu.
- **Skip Hearst Castle tour** — free Piedras Blancas elephant seals instead.
- **User chose** surf lesson + otter kayak + SoCal swimming. **Declined** ATV dunes.

**Hard constraints:**
1. **Monterey Car Week Aug 7–16** — do NOT book Monterey/Carmel/Big Sur lodging (sold out, $500+/night).
2. **Big Sur Hwy 1** is open, but **Rocky Creek Bridge has 24/7 one-way signal control through Aug 31** (≤15 min delay). Check Caltrans QuickMap the morning of Day 5.
3. **Fern Canyon** — free day-use reservation required May 15–Sep 15, **plus $12 CASH** (no cards).
4. **Pfeiffer Beach** — **$12 CASH** only.
5. Must be airside at LAX ~3 hours before the Aug 13 departure.

---

## 3. ⚠️ Water safety (must be preserved in any output)

**The northern half of this trip is not swimmable.** August water: **Oregon coast 52–59°F**, San Francisco ~58°F, Big Sur ~59°F. Wetsuit-only.

**Sneaker waves** on the Oregon/NorCal coast surge 150+ feet up the beach with no warning and have killed **24+ people on the West Coast since 2012**. Cold water causes loss of limb control within seconds; the waves carry sand and gravel that weigh clothing down.

**Rules to keep in any deliverable:** never turn your back on the ocean · stay off and away from beach logs · keep well up the sand at Cannon Beach, Samuel Boardman, and the Redwood beaches. Wading OK, swimming no.

**Swimming happens only in SoCal** — Santa Monica/Malibu ~**69°F**, lifeguarded (Day 6).

---

## 4. The itinerary

| Day | Date | Route | Sleep | Drive |
|---|---|---|---|---|
| 1 | Sat Aug 8 | Land SEA 12:27 → Link downtown → Pike Place, Great Wheel, water taxi → car 5 PM → Astoria dinner | Seaside/Cannon Beach | ~4 hr / 185 mi |
| 2 | Sun Aug 9 | Ecola SP → Cannon Beach → Neahkahnie → Cape Kiwanda → Cape Perpetua → Heceta Head | Bandon/Coos Bay | ~4.5 hr / 230 mi |
| 3 | Mon Aug 10 | Bandon → Samuel Boardman → Klamath Overlook → Fern Canyon → Avenue of the Giants | Garberville | ~5 hr / 250 mi |
| 4 | Tue Aug 11 | Golden Gate (daylight) → 🌊 **Elkhorn Slough otter kayak** 3–5 PM | Salinas | ~6 hr / 300 mi |
| 5 | Wed Aug 12 | Point Lobos → Garrapata → Bixby → Pfeiffer → Nepenthe → McWay → elephant seals → Morro Bay | Pismo Beach | ~5.5 hr / 250 mi |
| 6 | Thu Aug 13 | 🌊 **Malibu surf lesson** 10–12 → El Matador → Point Dume → 🌊 **Santa Monica swim** → *(Griffith if late flight)* → LAX | fly home | ~3 hr / 180 mi |

Full hour-by-hour timings are in `roadtrip_itinerary.md`; all 43 stops with coordinates and descriptions are in `trip_data.json`.

---

## 5. Budget (3 people)

| Item | Total | Per person |
|---|---|---|
| Flights | $777 | $259 *(+ fare diff to move to Aug 13)* |
| Rental car (6 days) | ~$798 | ~$266 |
| Gas | ~$230 | ~$77 |
| Lodging (5 nights, 1 room) | ~$700 | ~$233 |
| Food (6 days × ~$55) | ~$990 | ~$330 |
| Surf lesson | ~$270 | ~$90 |
| Otter kayak | ~$150 | ~$50 |
| Park & entry fees | ~$90 | ~$30 |
| Link, parking, tolls | ~$75 | ~$25 |
| **TOTAL** | **≈ $4,080** | **≈ $1,360** |

Range: $3,800 (frugal) – $4,500. Reference gas prices: OR ~$4.56/gal, CA ~$5.37/gal.

---

## 6. Good next tasks for Claude Code

- **Printable PDF / glovebox one-pager** — condensed day-by-day with addresses, phone numbers, confirmation numbers, and the cash-only reminders.
- **Budget spreadsheet** — editable, so real motel bookings replace estimates and the per-person split recalculates.
- **Offline-capable map** — cache the OSRM road geometry into the HTML as static GeoJSON so the route renders with no internet (big deal: cell service is genuinely bad in Big Sur and the Redwoods).
- **Booking tracker** — checklist of what's reserved vs. outstanding (see `must_book_ahead` in the JSON).
- **Mobile polish** — the sidebar is cramped on phones; a bottom-sheet layout would be better since this gets used in a car.
- **Add per-leg drive times** to the map from the OSRM response (it returns `duration`), instead of the hardcoded estimates in `DAYS[].drive`.

### Conventions to follow
- Keep the map a **single self-contained HTML file** (no build step) — it gets opened directly from a folder and shared.
- Keep the **straight-line fallback** whenever adding network-dependent features.
- Preserve the **water safety** section and the **cash-only** warnings in any new deliverable.
- Prices/conditions verified **mid-July 2026** — re-verify anything time-sensitive before the trip.
