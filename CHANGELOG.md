# 📜 Changelog — Top Tier Talent Group Job Map & Proximity Radar

All notable changes, fixes, and architectural upgrades for the **Top Tier Talent Group Proximity Radar & Active Job Map** are documented in this file.

---

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
