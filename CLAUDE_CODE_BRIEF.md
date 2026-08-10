# Claude Code Handoff — Pacific Coast Road Trip

Paste this whole file (plus `trip_data.json`) into Claude Code. It contains everything needed to continue the project with no prior conversation context.

---

## 1. What this project is

Planning a **Seattle → Los Angeles Pacific Coast road trip** for **3 guys, ages 19–20, first time on the coast**, and building an **interactive map** deliverable.

**Trip:** Aug 8–15, 2026 · 8 days, 7 nights · ~1,550 miles · fly into SEA, out of LAX.

### Files in this project
| File | What it is |
|---|---|
| `map.html` | **Main deliverable.** Self-contained interactive Leaflet map. Open in any browser. |
| `trip_data.json` | Machine-readable trip data: 8 days, 51 stops with coords, all constraints, budget. **Source of truth for the data.** |
| `roadtrip_itinerary.md` | Human-readable timed itinerary + costs + safety notes. |
| `CLAUDE_CODE_BRIEF.md` | This file. |

### How the map works (architecture)
Single self-contained HTML file. No build step, no framework.
- **Leaflet 1.9.4** + CARTO Voyager tiles, both from `cdnjs.cloudflare.com`.
- Two data arrays near the top of the `<script>`: `DAYS` (8 entries) and `STOPS` (51 entries). **Edit these to change the trip** — everything else derives from them.
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
awk 'f&&/<\/script>/{f=0} f{print} /<script>$/{f=1}' map.html > /tmp/app.js && node --check /tmp/app.js
```
Also sanity-check that latitudes within each day run roughly north→south — the coast portion is one continuous southbound drive on **Days 3–7**. Exceptions: **Day 1** ends inland at Ashford (southeast of Seattle, not on the coast); **Day 2** goes east into Mount Rainier NP first, then west to I-5 (night 2 actually ended inland at Kelso, WA); **Day 3** starts inland at Kelso and runs slightly *north*-west to Astoria before turning south down the coast; **Day 7** runs the LA coast east–west, so check longitude instead; **Day 8** is airport-only.

---

## 2. Locked-in facts (do not re-litigate)

**⚡ TRIP IS LIVE — actuals so far:** Days 1–2 happened. **Night 2 (Aug 9) actually ended at the Quality Inn in Kelso, WA** (I-5 exit 39) — they stopped short of the planned Seaside/Cannon Beach night after Rainier; no Astoria dinner. Day 3 was replanned from Kelso and then **compressed to essentials**: Astoria (Column + Riverwalk — recovering the missed Day 2 stop), Haystack Rock from the sand, Cape Kiwanda lunch, Thor's Well, Heceta Head; Ecola SP was cut (duplicate Haystack view, $10 + slow park road), Neahkahnie demoted to a drive-by, all other bonuses cut. Bandon arrival ~6:45 PM for a relaxed Face Rock sunset. Days 4–8 are unchanged.

**Travelers:** 3 people, 19–20 years old. **All under 21** — do not suggest bars/breweries as drinking activities (fine as restaurants). Budget-conscious: cheap motels, one room split 3 ways.

**Flights — both BOOKED. No rebooking advice is needed anymore.**
- **Out · Sat Aug 8:** DL2534 SAT 7:30 AM → LAX 8:33 AM, then DL1045 LAX 12:07 PM → **lands SEA 2:50 PM**. Delta, $124.
- **Back · Sat Aug 15:** **DL2568 LAX 10:59 AM → SAT 3:59 PM.** Morning flight — car back at LAX ~7:30 AM, airside by ~8:00 AM.

**Car:** Hertz — reservation **must be MODIFIED**: pickup **SEA ~3:45 PM Aug 8** (right after landing — the old 5 PM pickup and the car-less Link-light-rail downtown afternoon are dead), drop **LAX ~7:30 AM Aug 15**. Est **~$1,060 for 8 days** (was $663/6 — verify actual). Under-25 surcharge applies; **all three must be on the contract** to rotate drivers. Gas budgeted **~$290**.

**Decisions already made (with reasons):**
- **Trip extended to Aug 15 (8 days, 7 nights)** — bought a full **Mount Rainier day** and a full **LA day**, with the return flight booked for Sat morning.
- **Rainier day (Day 2):** sleep at the **Ashford gate night 1**, enter Nisqually **before 7 AM** (historically exempt from timed entry — **verify 2026 rules on nps.gov**), hike Paradise/Skyline, drive to the Oregon coast in the evening.
- **Sunrise side of Rainier skipped** — 2+ hr from Paradise; doesn't fit with the evening coast drive.
- **LA day (Day 7)** ends with **Griffith at sunset**, sleeping **near LAX night 7** — no more racing the beach day to a flight. Do not reintroduce a same-day flight after the beaches.
- **Olympic Peninsula skipped** — too far north, added ~200 miles.
- **No overnight-split drives** — they sleep in a bed every night and hit the Golden Gate in daylight. Do not reintroduce night driving.
- **Sleep Salinas, not Monterey** — Car Week.
- **Sleep Garberville, not Eureka** — avoids a 45-min backtrack to Avenue of the Giants.
- **Sleep Pismo night 6** — further south shortens the next morning to Malibu.
- **Skip Hearst Castle tour** — free Piedras Blancas elephant seals instead.
- **User chose** surf lesson + otter kayak + SoCal swimming. **Declined** ATV dunes.

**Hard constraints:**
1. **Mount Rainier** — enter the Nisqually gate **before 7 AM on Aug 9**; pre-7 AM entry has historically been exempt from timed-entry windows, but **VERIFY 2026 rules on nps.gov** and grab a reservation if required. $30/vehicle. No gas in the park; Grove of the Patriarchs remains closed.
2. **Monterey Car Week Aug 7–16** — do NOT book Monterey/Carmel/Big Sur lodging (sold out, $500+/night).
3. **Big Sur Hwy 1** is open, but **Rocky Creek Bridge has 24/7 one-way signal control through Aug 31** (≤15 min delay). Check Caltrans QuickMap the morning of Day 6.
4. **Fern Canyon** — free day-use reservation required **for Aug 11** (May 15–Sep 15 rule), **plus $12 CASH** (no cards).
5. **Pfeiffer Beach** — **$12 CASH** only.
6. **Return is a MORNING flight** — car back at LAX ~7:30 AM, **airside by ~8:00 AM Aug 15**.

---

## 3. ⚠️ Water safety (must be preserved in any output)

**The northern half of this trip is not swimmable.** August water: **Oregon coast 52–59°F**, San Francisco ~58°F, Big Sur ~59°F. Wetsuit-only.

**Sneaker waves** on the Oregon/NorCal coast surge 150+ feet up the beach with no warning and have killed **24+ people on the West Coast since 2012**. Cold water causes loss of limb control within seconds; the waves carry sand and gravel that weigh clothing down.

**Rules to keep in any deliverable:** never turn your back on the ocean · stay off and away from beach logs · keep well up the sand at Cannon Beach, Samuel Boardman, and the Redwood beaches. Wading OK, swimming no.

**Swimming happens only in SoCal** — Santa Monica/Malibu ~**69°F**, lifeguarded (Day 7).

---

## 4. The itinerary

| Day | Date | Route | Sleep | Drive |
|---|---|---|---|---|
| 1 | Sat Aug 8 | Land SEA 2:50 → car ~3:45 → Pike Place, Great Wheel → Kerry Park sunset → drive to the Rainier gate | Ashford | ~2.5 hr / 100 mi |
| 2 | Sun Aug 9 | 🏔 **Mount Rainier**: Nisqually gate 6 AM → Christine + Narada Falls → **Paradise Skyline Trail** 7:15–1 → Reflection Lakes → out by ~5:30 | **ACTUAL: Quality Inn, Kelso WA** (stopped short of the coast) | ~4 hr / ~130 mi |
| 3 | Mon Aug 10 | **Compressed:** Kelso 7:15 → Astoria Column + Riverwalk (missed Day 2 stop, recovered) → Haystack Rock from the sand → Cape Kiwanda lunch → Thor's Well → Heceta Head. Cut: Ecola, Hug Point, Sea Lion Caves, sandboarding; Neahkahnie drive-by only | Bandon/Coos Bay ~6:45 PM | ~5.5 hr / ~295 mi |
| 4 | Tue Aug 11 | Bandon → Samuel Boardman → Klamath Overlook → Fern Canyon → Avenue of the Giants | Garberville | ~5 hr / 250 mi |
| 5 | Wed Aug 12 | Golden Gate (daylight, ~12:30) → 🌊 **Elkhorn Slough otter kayak** 3–5 PM | Salinas | ~6 hr / 300 mi |
| 6 | Thu Aug 13 | Point Lobos → Garrapata → Bixby → Pfeiffer → Nepenthe → McWay → elephant seals → Morro Bay | Pismo Beach | ~5.5 hr / 250 mi |
| 7 | Fri Aug 14 | 🌊 **Malibu surf lesson** 10–12 → El Matador → Point Dume → 🌊 **Santa Monica swim** 2:30 → Venice → **Griffith sunset** 6:30 | near LAX (El Segundo/Inglewood) — gas up tonight | ~4 hr / 200 mi |
| 8 | Sat Aug 15 | Car back 7:30 AM → airside ~8:00 → DL2568 10:59 AM → SAT 3:59 PM | fly home | ~15 mi |

Full hour-by-hour timings are in `roadtrip_itinerary.md`; all 51 stops with coordinates and descriptions are in `trip_data.json`.

---

## 5. Budget (3 people)

| Item | Total | Per person |
|---|---|---|
| Flights | $777 | $259 *(update with actual Aug 15 fare)* |
| Rental car (8 days) | ~$1,060 | ~$353 |
| Gas | ~$290 | ~$97 |
| Lodging (7 nights, 1 room) | ~$980 | ~$327 |
| Food (8 days × ~$55) | ~$1,320 | ~$440 |
| Surf lesson | ~$270 | ~$90 |
| Otter kayak | ~$150 | ~$50 |
| Park & entry fees (incl. Rainier $30) | ~$120 | ~$40 |
| Transit, parking, tolls | ~$75 | ~$25 |
| **TOTAL** | **≈ $5,040** | **≈ $1,680** |

Range: $4,700 (frugal) – $5,500. Reference gas prices: OR ~$4.56/gal, CA ~$5.37/gal.

---

## 6. Good next tasks for Claude Code

*(Note: the trip changed on Aug 2 — extended to Aug 8–15 with a Rainier day and a full LA day. Any older artifact that says 6 days / Aug 13 / "rebook the return" is stale.)*

- **Printable PDF / glovebox one-pager** — condensed day-by-day with addresses, phone numbers, confirmation numbers, and the cash-only reminders.
- **Budget spreadsheet** — editable, so real motel bookings replace estimates and the per-person split recalculates.
- **Offline-capable map** — cache the OSRM road geometry into the HTML as static GeoJSON so the route renders with no internet (big deal: cell service is genuinely bad in Big Sur, the Redwoods, and Mount Rainier NP).
- **Booking tracker** — checklist of what's reserved vs. outstanding (see `must_book_ahead` in the JSON; the Ashford night-1 room and the Hertz modification are the urgent ones).
- **Mobile polish** — the sidebar is cramped on phones; a bottom-sheet layout would be better since this gets used in a car.
- **Add per-leg drive times** to the map from the OSRM response (it returns `duration`), instead of the hardcoded estimates in `DAYS[].drive`.

### Conventions to follow
- Keep the map a **single self-contained HTML file** (no build step) — it gets opened directly from a folder and shared.
- Keep the **straight-line fallback** whenever adding network-dependent features.
- Preserve the **water safety** section and the **cash-only** warnings in any new deliverable.
- Prices/conditions verified **mid-July 2026** — re-verify anything time-sensitive before the trip.
