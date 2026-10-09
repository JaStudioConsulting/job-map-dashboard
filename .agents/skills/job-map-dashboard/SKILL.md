---
name: job-map-dashboard
description: Maintenance, data ingestion, geocoding, and deployment rules for the Top Tier Talent Group (TTTG) Job Map & Proximity Radar dashboard.
---

# Job Map & Proximity Radar Maintenance Skill

This skill guides AI agents on maintaining, updating, and debugging the **Top Tier Talent Group (TTTG) Job Map & Proximity Radar** dashboard.

## 1. Project Location & Repository
* **Local Workspace Directory:** `/Users/TTTG/Documents/Mother_Work/job-map-dashboard`
* **GitHub Repository:** `https://github.com/JaStudioConsulting/job-map-dashboard.git` (`main` branch)
* **Live Deployment:** `https://jastudioconsulting.github.io/job-map-dashboard/`
* **Passcode:** `TTTG2026`

## 2. Architecture & Tech Stack
* **Self-Contained SPA (`index.html`):** Single-file architecture requiring zero build steps or bundlers.
* **Libraries:**
  - React 18 & ReactDOM via `unpkg.com`
  - Tailwind CSS via CDN (`cdn.tailwindcss.com`)
  - Leaflet.js (`unpkg.com/leaflet@1.9.4`) with OpenStreetMap tiles
  - Babel Standalone (`@babel/standalone`) for JSX compilation in-browser
* **Theme & UI Design:**
  - Warm executive stone & charcoal palette (`#FAF9F6` background, `#2B2723` stone buttons, emerald compensation badges).
  - Consistent with JA Studio Consulting / TTTG organizational style.

## 3. Data Synchronization Pipeline
* **Google Sheet Source:**
  - Sheet ID: `1RexJFAEWRwNjzeGThutrA0zMObu2g4r3q0Vu0uiDbQs`
  - Sheet Tab: `Jobs`
* **Ingestion Strategy:**
  1. `fetchSheetViaJSONP(sheetId, 'Jobs')`: Uses Google Visualization API (`/gviz/tq?sheet=Jobs&tqx=responseHandler:...`) via dynamic script injection. This guarantees no CORS errors in static web environments.
  2. Fallback to direct CSV fetch with custom quote-aware CSV parser `parseCSV(text)`.
  3. Offline fallback `MOCK_JOBS` if network fails.
* **Filter Rule:**
  - ONLY rows where `(job['Status'] || '').toLowerCase() === 'active'` are ingested.

## 4. Location & Geocoding Rules
* **Multi-Location Openings:**
  - Delimiters recognized: `/`, `;`, `|`, `\n`, `\r`, `&`, `and`.
  - Splitting logic: `splitLocationString(rawLoc)` detects multiple cities or sites.
  - Multi-site jobs are expanded into individual branch entries with `isMultiLocation: true`, `locationIndex`, `totalLocations`, and `allLocations`.
  - Each branch gets an independent map pin and candidate distance ranking.
* **Coordinate Resolution:**
  - `CITY_COORDINATES`: Preloaded lookup dictionary for Ontario & North American hubs.
  - Persistent Geocache: Stored in `localStorage.getItem('tttg_geo_cache')`.
  - Background Exact Geocoder: `geocodeExactAddresses(jobList)` queries OpenStreetMap Nominatim for exact street addresses (with 650ms rate limit) and caches results.
* **Uber-Style Live Address Autocomplete:**
  - Debounced typeahead queries OSM Photon API (`https://photon.komoot.io/api/?q=...&limit=5&lat=43.7&lon=-79.4`) biased towards Ontario / North America.
  - Floating dropdown with mouse and keyboard navigation (`↑`/`↓` + `Enter`).

## 5. Candidate Distance Calculation
* **Haversine Formula:** `calculateDistance(lat1, lon1, lat2, lon2, unit)` calculates straight-line distance in `km` or `mi`.
* **Proximity Sorting:** When `searchCoords` is active, jobs sort from closest to farthest.

## 6. Safety & Deployment Guidelines
* **Safety Persona Rule:** Always ask user permission before running terminal commands unless user says "yolo".
* **Deployment Workflow:**
  1. Make edits to `/Users/TTTG/Documents/Mother_Work/job-map-dashboard/index.html` or `README.md`.
  2. Ask permission to execute git commands:
     ```bash
     git add .
     git commit -m "feat/fix: description of change"
     git push origin main
     ```
  3. Verify deployment live at `https://jastudioconsulting.github.io/job-map-dashboard/`.
