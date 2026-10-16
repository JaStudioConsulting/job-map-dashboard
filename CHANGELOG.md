# 📜 Changelog — Top Tier Talent Group Job Map & Proximity Radar

All notable changes, fixes, and architectural upgrades for the **Top Tier Talent Group Proximity Radar & Active Job Map** are documented in this file.

---

## [Unreleased] / v2.7.0 - 2026-10-16

### Three-part flow: job list, map, matches
- **Left panel** shows the job list. Picking a job collapses it to that job card plus role details, with a "Back to all roles" link.
- **Right panel** lists candidates matched to the picked job, ranked title match first, then industry, then city-level distance. Replaces the floating job drawer.
- **Radius dial** (10 to 150 km, plus All, default 50 km). Title matches outside the radius, or with no location, stay listed. Candidate distances are labelled approximate, city level. Province-level rows show "Province-level location" instead of a number.
- **Mobile** gets a third tab (Map / Job / Matches) once a job is picked.

### Candidate pins
- **No more swirl.** Each candidate gets a stable scattered point inside their city (7 km, 40 km for province-only rows), seeded from name, title, company and location so pins never jump between loads. Lake Ontario shoreline guard keeps GTA, Hamilton and Niagara pins on land.
- **Calm clusters.** Spiderfy and cluster animation off. Clustering stops at street zoom so every pin shows.
- **On-demand loading.** Candidates are hidden until a job is picked or a location is searched. Only pins inside the current map view are drawn (capped at 300) and they reload on pan or zoom.
- **City coverage.** Candidate locations now parse the first place in free text ("Brampton, ON | open to relocation"). About 160 more cities added to the built-in table from a one-time city-level geocode, so about 1,377 of 1,511 candidates now get a city pin (was about 960). Unmatched cities geocode once in the background and cache locally. Nothing is written back to the sheet.

### Look and feel
- Arial/Calibri only (Inter and JetBrains Mono removed). Black text, no colored badges.
- Search bar restyled: white field, 1px border, 10px radius, single pin icon, "Search a city, postal code, or address". Owners, radius, km/mi and Region sit on one row at one height. Region is now a single select.
- Header and map controls simplified to one button style, emoji icons replaced.

### Fixes
- Map pin effects were declared before the memoized lists they depended on, so filter changes did not always redraw pins. Moved below `matchedCandidates`.
- Picking a job on the mobile List tab no longer throws a Leaflet fly error on the hidden map.
- Location suggestions close when a search is submitted.

## [Unreleased] / v2.6.0 — 2026-10-13

### 🎨 Visual & Marker Upgrades
- **Transparent Emblems (No Background Bubble):**
  - Removed circular white badge containers and heavy borders from map markers.
  - Candidate pins now render as the standalone Top Tier Talent Group laurel wreath logo (`candidate-pin.png`) floating with a subtle drop-shadow (`drop-shadow-[0_2px_4px_rgba(0,0,0,0.35)]`).
  - Job pins render as standalone `💰` emoji markers with matched drop-shadows.
- **Downtown Toronto Starting View:**
  - Map default coordinates updated to `[43.6625, -79.3900]` at zoom `13` (focusing on the University of Toronto / Queen's Park / Downtown corridor).
  - "Toronto, ON" focus button and Ontario region filter re-aligned to this optimal street-level perspective.
- **Streamlined Map Controls:**
  - Removed "My Location" GPS button to keep top-right navigation clean, executive, and minimal. Retained **Toronto, ON** and **All Pins** quick actions.

### 🌿 Candidate Geographic Dispersion
- **Golden Angle Spiral Jitter:**
  - Candidates sharing a common city hub (e.g. Toronto) are automatically and deterministically spread using a 137.5° golden angle spiral outward from 0.55 km to ~3.8 km.
  - Prevents marker stacking while maintaining realistic neighborhood placement.
  - Added northern latitude bias to keep dispersed Toronto pins on land (away from Lake Ontario).

### 🛡️ Bug Fixes & Code Stability
- **JSX Compilation Fix:**
  - Restored missing closing `</div>` tag on the candidate card container in the candidate city drawer that was causing in-browser Babel transpile failures.
- **Defensive Coordinate Guards:**
  - Added strict array and `NaN` validation in `spreadCandidates` and Leaflet marker loops to guarantee zero crashes on unexpected data rows.

---

## [v2.5.0] — 2026-10-12

### 🔍 Search & Autocomplete
- **Uber-Style Typeahead Autocomplete:**
  - Integrated OSM Photon autocomplete API for instant street, postal code, and city suggestions.
  - Keyboard navigation (`↑`, `↓`, `Enter`, `Esc`) and click-outside dismissal.
  - Persistent local geocache (`localStorage.tttg_geo_cache`) for zero-latency lookups on previously visited locations.
- **Smart Multi-Stage Geocoding:**
  - Full address, partial address, and regional hub fallback stages.

### 📐 Workspace & UI
- **Draggable Split-Pane Sidebar:**
  - Resizable sidebar with persistent width memory in `localStorage`.
  - Double-click to reset width to default 320px.
- **Plain iOS Region Checkboxes:**
  - Region toggles for `Show All`, `Ontario Only`, `Canada`, and `Global`.

---

## [v2.0.0] — 2026-10-11

### 💼 Candidate Matching & Multi-Location Split
- **Multi-Location Job Splitting:**
  - Automatically splits openings with multiple branches (`/`, `;`, `|`, `&`) into separate map markers with individual distance ranking.
- **Proximity Candidate Matching Drawer:**
  - Slide-out candidate matching drawer showing candidates within 25 km, 50 km, 100 km, or 250 km of any selected job opening.
- **One-Click Pitch Generators:**
  - Formatted recruiter pitch copies for jobs and candidates ready for instant outreach.
- **Recruiter Passcode Gatekeeper:**
  - Added `TTTG2026` team passcode protection and search engine `noindex, nofollow` headers.
