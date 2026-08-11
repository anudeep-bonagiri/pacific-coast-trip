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

**⚡ TRIP IS LIVE — actuals so far:** Days 1–2 happened. **Night 2 (Aug 9) actually ended at the Quality Inn in Kelso, WA** (I-5 exit 39) — they stopped short of the planned Seaside/Cannon Beach night after Rainier; no Astoria dinner. **Day 3 (Mon) was re-scoped mid-drive to a full push into California**, ending in **Eureka ~11:45 PM–12:30 AM** (Crescent City is the written-in bail-out if a driver is fading; 2–4 AM is the danger window). **Samuel Boardman is cut** — passed in the dark. The new regional focus is the **Mendocino/Fort Bragg coast**: Day 4 runs Eureka → Avenue of the Giants + Rockefeller Forest → Garberville lunch → Chandelier Drive-Thru Tree → **Leggett Hill (Hwy 1's winding hour, daylight-only hard rule)** → Westport → Glass Beach → MacKerricher/Laguna Point (free, the priority park) → Point Cabrillo → Mendocino Headlands sunset → sleep **Fort Bragg**. Day 5 exits east on **CA-20 to Willits at a hard 7:00 AM** (Hwy 1 south would add 2.5 hr and blow the 3 PM kayak). **Also cut: Fern Canyon and Klamath River Overlook** — both ~45 min *north* of Eureka, so they'd mean a 3.5-hr backtrack and a dark Leggett Hill; the Aug 11 Fern Canyon reservation was free, so nothing lost but the $12 cash. **Garberville night cancelled.** The Hertz drop remains **Friday Aug 14 by 9 PM at LAX** (gas before Griffith, leave the observatory 7:50); Day 8 is shuttle-only. Days 6–8 otherwise unchanged.

**Travelers:** 3 people, 19–20 years old. **All under 21** — do not suggest bars/breweries as drinking activities (fine as restaurants). Budget-conscious: cheap motels, one room split 3 ways.

**Flights — both BOOKED. No rebooking advice is needed anymore.**
- **Out · Sat Aug 8:** DL2534 SAT 7:30 AM → LAX 8:33 AM, then DL1045 LAX 12:07 PM → **lands SEA 2:50 PM**. Delta, $124.
- **Back · Sat Aug 15:** **DL2568 LAX 10:59 AM → SAT 3:59 PM.** The car went back Friday night — Saturday is hotel shuttle ~8:30 AM, airside ~9.

**Car:** Hertz — picked up SEA ~3:45 PM Aug 8; **drop UPDATED to LAX Friday Aug 14 by 9 PM** (7 days — verify the adjusted total vs the ~$1,060/8-day estimate). Under-25 surcharge applies; **all three are on the contract** to rotate drivers. Gas budgeted **~$290**.

**Decisions already made (with reasons):**
- **Trip extended to Aug 15 (8 days, 7 nights)** — bought a full **Mount Rainier day** and a full **LA day**, with the return flight booked for Sat morning.
- **Rainier day (Day 2):** sleep at the **Ashford gate night 1**, enter Nisqually **before 7 AM** (historically exempt from timed entry — **verify 2026 rules on nps.gov**), hike Paradise/Skyline, drive to the Oregon coast in the evening.
- **Sunrise side of Rainier skipped** — 2+ hr from Paradise; doesn't fit with the evening coast drive.
- **LA day (Day 7)** ends with **Griffith at sunset**, sleeping **near LAX night 7** — no more racing the beach day to a flight. Do not reintroduce a same-day flight after the beaches.
- **Olympic Peninsula skipped** — too far north, added ~200 miles.
- **Night driving is now a bounded, deliberate exception** (Mon Aug 10 into Eureka), not a habit — rotate drivers, Crescent City is the bail-out. They still sleep in a bed every night and hit the Golden Gate in daylight.
- **Sleep Salinas, not Monterey** — Car Week.
- ~~Sleep Garberville~~ — **superseded Aug 10**: the trip now sleeps Eureka (Mon) and Fort Bragg (Tue).
- **Sleep Pismo night 6** — further south shortens the next morning to Malibu.
- **Skip Hearst Castle tour** — free Piedras Blancas elephant seals instead.
- **User chose** surf lesson + otter kayak + SoCal swimming. **Declined** ATV dunes.

**Hard constraints:**
0a. **Leggett Hill (Hwy 1 from Leggett to the coast) must be driven in DAYLIGHT** — reach Leggett by ~1:30 PM Tuesday.
0b. **Day 5: HARD 7:00 AM departure from Fort Bragg via CA-20** — the 3 PM Elkhorn Slough kayak is ~6 hr away and Hwy 1 south is not an option.
0. **Car back at LAX FRIDAY Aug 14 by 9:00 PM** (modified drop) — gas before Griffith, leave the observatory 7:50 PM. Saturday is car-less: hotel airport shuttle ~8:30 AM for the 10:59 flight.
1. **Mount Rainier** — enter the Nisqually gate **before 7 AM on Aug 9**; pre-7 AM entry has historically been exempt from timed-entry windows, but **VERIFY 2026 rules on nps.gov** and grab a reservation if required. $30/vehicle. No gas in the park; Grove of the Patriarchs remains closed.
2. **Monterey Car Week Aug 7–16** — do NOT book Monterey/Carmel/Big Sur lodging (sold out, $500+/night).
3. **Big Sur Hwy 1** is open, but **Rocky Creek Bridge has 24/7 one-way signal control through Aug 31** (≤15 min delay). Check Caltrans QuickMap the morning of Day 6.
4. ~~**Fern Canyon**~~ — **DROPPED** (it's north of Eureka; the plan now runs south). The free Aug 11 reservation is forfeited, and the $12 cash is no longer needed there.
5. **Pfeiffer Beach** — **$12 CASH** only.
6. **Return is a MORNING flight** — DL2568 at 10:59 AM Aug 15; no car that morning (see constraint 0), hotel shuttle ~8:30, **airside by ~9:00 AM**.

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
| 3 | Mon Aug 10 | **Oregon speed-run:** Kelso 9:00 → Haystack ✅ → Tillamook Creamery ✅ → Thor's Well 4:15 → Heceta 5:00 → Coos Bay dinner/fuel 6:40 → Brookings 9:15 → **big night push into California** (Crescent City = bail-out) | **Eureka, CA ~11:45 PM** | ~10 hr / ~490 mi |
| 4 | Tue Aug 11 | **The payoff:** Eureka 9:00 → Avenue of the Giants/Founders Grove → Rockefeller Forest (Mattole Rd) → Garberville lunch → Chandelier Drive-Thru Tree → **★ Leggett Hill, Hwy 1's winding hour (daylight only)** → Westport → Glass Beach → MacKerricher/Laguna Point → Point Cabrillo → Mendocino Headlands sunset | **Fort Bragg ~9 PM** | ~5 hr / ~200 mi |
| 5 | Wed Aug 12 | **HARD 7:00 AM** out of Fort Bragg → **CA-20 east to Willits** → US-101 → Golden Gate/Battery Spencer 12:00 (20 min max) → 🌊 **Elkhorn Slough kayak 3–5 PM** | Salinas | ~6.5 hr / ~330 mi |
| 6 | Thu Aug 13 | Point Lobos → Garrapata → Bixby → Pfeiffer → Nepenthe → McWay → elephant seals → Morro Bay | Pismo Beach | ~5.5 hr / 250 mi |
| 7 | Fri Aug 14 | 🌊 **Malibu surf lesson** 10–12 → El Matador → Point Dume → 🌊 **Santa Monica swim** 2:30 → Venice → gas up → **Griffith sunset** 6:30, leave 7:50 → **Hertz LAX by 9 PM** | near LAX via hotel shuttle | ~4.5 hr / ~215 mi |
| 8 | Sat Aug 15 | No car (returned Friday 9 PM) → hotel shuttle ~8:30 → airside ~9:00 → DL2568 10:59 AM → SAT 3:59 PM | fly home | shuttle |

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
