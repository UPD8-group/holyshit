# HolyShit.app — Next Session Briefing
*Updated after big session — March 2026*

---

## Current Status (as of this session)
- Live at holyshit.app
- **50/50 code audit passed** — cleanest version yet
- State-based JSON loading (8 files, one per state)
- 4-button bottom nav: 🚗 Drive | 🏃 Walk/Run | 🚴 Cycle | 💩 Near Me
- Map type FAB + Locate FAB stacked on right (Google Maps style)
- Splash screen with tap-to-start (fixes iOS location permission)
- Auto-start when coming from landing page (no double tap)
- Map opens centred on user location, falls back to Canberra
- Satellite view as default
- Map follows user during active navigation
- Cross-border state reload (e.g. ACT → NSW mid-session)

### Files currently deployed:
- `app.html` — main app
- `index.html` — landing page (updated: "toilets near you", no "public toilets" claim)
- `privacy.html` — privacy policy (NEW)
- `terms.html` — terms of use (NEW)
- `toilets-act.json` — 1,549 records, 134KB
- `toilets-nsw.json` — 28,135 records, 2.3MB
- `toilets-vic.json` — 15,823 records, 1.4MB
- `toilets-qld.json` — 15,045 records, 1.2MB
- `toilets-sa.json` — 5,662 records, 467KB
- `toilets-wa.json` — 8,083 records, 645KB
- `toilets-tas.json` — 2,541 records, 210KB
- `toilets-nt.json` — 947 records, 72KB

---

## Priority 1 — Expand the Database (MAIN TASK NEXT SESSION)

**The source: Overture Maps** — free, worldwide, no licensing restrictions.
Run by the Overture Maps Foundation (Apple, Meta, Microsoft, Amazon).
Download once, store yourself, no ongoing API costs. Worldwide coverage.

### Step 1 — Run this on your laptop at home first:
```bash
pip install overturemaps
overturemaps download --bbox=113.3,-43.7,153.6,-10.7 --type=place -f geojson -o overture-au-places.geojson
```
Takes 10-15 mins. Produces a large GeoJSON file of all Australian places.
Then upload it here and I'll process it into the database.

### Step 2 — I'll filter and tag using the stranger rule:
**If a stranger can walk in without showing a card or paying first — it's in.**

#### Always Open (no icon)
- Dedicated public toilets ✅ already have
- Highway rest stops ✅ already have
- Petrol/service stations (with toilets=yes tag)
- Train stations and bus interchanges

#### Daytime Only ☀️
- Shopping centres
- Bunnings and hardware stores
- McDonald's and fast food (dining room only)
- Supermarkets (Woolies, Coles, Aldi)
- Public libraries
- Council/government shopfronts
- Community centres
- Sports clubs and ovals
- Hospitals and medical centres (public areas)
- Beaches and surf clubs
- National parks and rest stops
- Universities and TAFE campuses
- Caravan parks (some public access)

#### Exclude
- Hotels (unreliable lobby access)
- Costco (membership required)
- Gyms (membership required)
- Pubs and clubs (patron access only)

### Step 3 — Rebuild all 8 state JSON files with ☀️ tags

---

## Priority 2 — Night Mode

**Logic:** Device clock, automatic, no user input.

- **6am–10pm:** Show everything, ☀️ on daytime venues
- **10pm–6am:** Night mode — only 24hr dedicated public toilets

**Toast when opening in night mode:**
> "🌙 Night mode — only showing 24hr public facilities"

---

## Priority 3 — Location Status Icons

- ☀️ = Daytime only
- 🚫 = Not reliably accessible  
- No icon = Always open

---

## Priority 4 — Analytics Dashboard

Anonymous, no cookies:
- User region
- Markers tapped
- Device type
- Session length
- Near Me usage

**Stack:** Netlify Analytics (free) + Supabase (already have for Say G'day)
**Goal:** 10,000 MAU to show Garmin/Strava

---

## Priority 5 — Monetisation (low urgency)

- "Buy us a coffee ☕" tip jar (Ko-fi or Buy Me a Coffee)
  - Copy: "💩 Saved your run? Buy us a coffee ☕"
- Longer term: Garmin/Strava pitch with real numbers
- Portfolio play: sell whole UPD8 portfolio

---

## UPD8 App Engine — The Bigger Picture

HolyShit is the engine. Every other app = reskin:
- Same map, FABs, nav, GPS, directions, street view, reporting
- Swap dataset + pin colour + brand

**Portfolio:** DogBowl, BabyChange, WattNow, LittleLibrary, 3AMFood, PublicBBQ

**whilecharging.app** — Register this domain ASAP!
Find toilets/food/coffee while your EV charges. Captive 20-30 min audience.
Works for ferries, airports, waiting rooms too.

---

## Known Issues / Watch List
- `netlify.toml` causes pin rendering issues — left out for now
- No Supabase yet — Report button uses Netlify Forms only
- Leaderboard, Settings, Share Run = SOON badges, not built yet
- Route planner + run tracker in codebase but no nav trigger (intentional)

---

## Bugs Fixed This Session
1. Orphaned JS before DOCTYPE — broke Samsung Internet
2. Infinite recursion in setMode() — crashed on mode tap
3. startCenter out of scope in loadData() — allLoos always empty (Bren's bug!)
4. cycleMapType() vs cycleMapMode() mismatch — map type FAB did nothing
5. cycleMapMode() referenced missing DOM elements
6. Race condition: auto-start before Google Maps loaded on iOS
7. findNearest() silent when data empty
8. updateTravelModeLabel() referenced missing nav elements
9. Mode buttons not highlighted on load
10. Toast overflow on small screens
11. State data reloaded every GPS tick — now cached
12. No cross-border data reload — now auto-detects
