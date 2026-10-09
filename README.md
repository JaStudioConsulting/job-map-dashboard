# 🗺️ Top Tier Talent Group — Job Map & Proximity Radar (v2.4)

An interactive single-page web dashboard built for **Top Tier Talent Group** to visualize active job openings across Ontario and North America, calculate candidate commute proximity in real-time, and streamline recruiter pitches.

* **Live URL:** [https://jastudioconsulting.github.io/job-map-dashboard/](https://jastudioconsulting.github.io/job-map-dashboard/)
* **Recruiter Passcode:** `TTTG2026`
* **Source Code:** [https://github.com/JaStudioConsulting/job-map-dashboard](https://github.com/JaStudioConsulting/job-map-dashboard)

---

## 📋 Table of Contents
1. [Recruiter Quick-Start & Workflow](#-recruiter-quick-start--workflow)
2. [Google Sheet Data Entry Guide (Crucial)](#-google-sheet-data-entry-guide)
   - [Column Specifications](#column-specifications)
   - [Single City / Standard Formatting](#1-single-city-standard)
   - [Multi-Location Openings (e.g., BFG)](#2-multi-location-openings)
   - [Exact Street Addresses & Postal Codes](#3-exact-street-addresses--postal-codes)
3. [Architecture & Technical Design](#-architecture--technical-design)
4. [Deployment & GitHub Pages Maintenance](#-deployment--github-pages-maintenance)
5. [Troubleshooting & FAQs](#-troubleshooting--faqs)

---

## 🎯 Recruiter Quick-Start & Workflow

1. **Accessing the Dashboard:**
   - Open the live link: `https://jastudioconsulting.github.io/job-map-dashboard/`
   - Enter team passcode: `TTTG2026`.
2. **Finding the Closest Roles to a Candidate:**
   - Type candidate's city, town, or postal code in the **Candidate Location** search box (e.g. `London, ON` or `N6A 1A1`).
   - Click **Find Matches** or press Enter.
   - The job list instantly re-ranks with nearest roles at the top and displays exact distance (`📍 12.4 km away`).
3. **Filtering by Recruiter or Commute Radius:**
   - Filter by Recruiter owner (`Ja`, `Sarah`, `Candice`, `Lyn`, etc.).
   - Limit radius (`Within 25 km`, `50 km`, `100 km`, `250 km`).
   - Toggle between **km** and **mi**.
4. **Copying Candidate Pitches:**
   - Click **📋 Copy Pitch** on any card or inside the detail drawer.
   - Automatically copies a formatted brief to your clipboard ready for candidate calls or emails.

---

## 📊 Google Sheet Data Entry Guide

The dashboard synchronizes live with the **`Jobs`** tab in the team Google Sheet:
* **Sheet ID:** `1RexJFAEWRwNjzeGThutrA0zMObu2g4r3q0Vu0uiDbQs`
* **Tab Name:** `Jobs`

### Column Specifications

| Column Header | Required? | Example Value | Description |
| :--- | :---: | :--- | :--- |
| **`Job ID`** | Yes | `3677925` | Unique ATS or system identifier |
| **`Job Title`** | Yes | `Lead Tool & Die Maker` | Clean title of the open role |
| **`Company`** | Yes | `Magna International` | Client or employer name |
| **`Location`** | **Yes** | `Windsor, ON` *(see formats below)* | Role branch or work location |
| **`Compensation`** | Recommended | `$38 - $42/hr` or `$110,000/yr` | Displays as top-right salary badge |
| **`Owners`** | Recommended | `Ja, Sarah` | Recruiter names (powers filter dropdown) |
| **`Status`** | **Yes** | `Active` | Only `Active` rows appear on the dashboard |
| **`Job Description`** | Optional | Full text overview... | Shown in slide-out detail drawer |
| **`Client Contact`** | Optional | `Melanie Francis` | Contact person for the role |

> [!IMPORTANT]
> **Status Check:** If a job's status is set to `Closed`, `Draft`, `Inactive`, or left blank, the dashboard will automatically omit it. To make a role visible, set `Status` = `Active`.

---

### 1. Single City / Standard
Enter city and province/state code:
* `Windsor, ON`
* `London, ON`
* `Belleville, ON`
* `Anaheim, CA`
* `Detroit, MI`

---

### 2. Multi-Location Openings (e.g., 4 openings for BFG)
If one position has multiple hiring sites, separate the locations in the `Location` column with **`/`**, **`;`**, **`|`**, or **`&`**:
* **Example A (Slash):** `Windsor, ON / London, ON / Chatham, ON / Kitchener, ON`
* **Example B (Semicolon):** `Mississauga, ON; Brampton, ON; Vaughan, ON`
* **Example C (Pipe):** `Detroit, MI | Troy, MI | Warren, MI`

#### How the Dashboard Handles Multi-Locations:
1. **Multiple Map Pins:** Drops an interactive pin for every branch on the map.
2. **Individual Distance Ranking:** When a recruiter searches a candidate in London, ON, the *London branch* ranks at the top (#1, 4 km away), while Windsor and Kitchener rank at their respective distances.
3. **Smart Badging:** Displays `Site X of 4` badge on the card and drawer.
4. **Enhanced Pitch:** Copying the pitch includes the branch context:
   > `📍 Location: London, ON (Site 2 of 4 - Other sites: Windsor, ON, Chatham, ON, Kitchener, ON)`

---

### 3. Exact Street Addresses & Postal Codes
If you have the exact facility address or postal code, enter it directly in the `Location` column:
* `4500 Rhodes Dr, Windsor, ON N8W 5K5`
* `100 King St W, Toronto, ON M5X 1A9`
* `3300 E Guasti Rd, Ontario, CA 91761`

#### How the Dashboard Handles Exact Addresses:
1. **Instant City Fallback:** Detects the city immediately to drop an initial pin with zero load delay.
2. **Background Precision Geocoder:** Asynchronously queries OpenStreetMap Nominatim for exact rooftop GPS coordinates.
3. **Local Geocache:** Stores coordinates in browser `localStorage` (`tttg_geo_cache`) so repeated visits load instantly with 0 API calls.

---

## 🛠️ Architecture & Technical Design

The dashboard is built as a self-contained, high-reliability Single-Page Application (SPA):

* **Runtime:** Zero-build React 18 & ReactDOM loaded via CDN.
* **Styling:** Tailwind CSS CDN customized with Top Tier Talent's executive stone & charcoal theme (`#FAF9F6` background, stone borders, emerald compensation pills).
* **Mapping Engine:** Leaflet.js with standard OpenStreetMap tile layers for 100% global coverage.
* **Dual Ingestion Engine:**
  1. **Primary:** Google Visualization JSONP API (`/gviz/tq?sheet=Jobs&tqx=responseHandler:...`) — completely bypasses browser CORS restrictions on static file and GitHub Pages environments.
  2. **Secondary Fallback:** Direct CSV fetch with custom quote-aware CSV parser.
  3. **Offline Fallback:** Cached sample dataset if network is unavailable.
* **Security:** Recruiter gatekeeper (`TTTG2026`) stored in `localStorage` (`tttg_radar_auth`), with robots `noindex, nofollow` to prevent indexing on public search engines.

---

## 🚀 Deployment & GitHub Pages Maintenance

### File Structure:
```
job-map-dashboard/
├── index.html       # Complete application (HTML + React + Tailwind + Leaflet)
├── package.json     # Metadata
├── README.md        # Comprehensive documentation & data guide
└── .agents/
    └── skills/
        └── job-map-dashboard/
            └── SKILL.md  # AI Agent maintenance skill
```

### Making Changes & Deploying:
The site is hosted directly on GitHub Pages from the `main` branch root (`/`). Any push to `main` automatically triggers a live rebuild in ~30 seconds.

```bash
cd /Users/TTTG/Documents/Mother_Work/job-map-dashboard

# Check status
git status

# Add and commit updates
git add .
git commit -m "feat: your update message"

# Deploy live to GitHub Pages
git push origin main
```

---

## ❓ Troubleshooting & FAQs

### Q: I added a job to Google Sheets, but it is not appearing on the map.
1. **Check Status Column:** Ensure the `Status` column in Google Sheets is set to `Active` (case-insensitive).
2. **Check Location Column:** Ensure the `Location` column has a valid city/province (e.g. `Windsor, ON`) or address.
3. **Click Refresh:** Click the **🔄 Refresh** button in the dashboard header to fetch fresh data from Google Sheets.
4. **Bypass Browser Cache:** Append `?v=new` to the URL or press `Ctrl + F5` / `Cmd + Shift + R`.

### Q: How do I change the recruiter passcode?
Open `index.html`, locate line ~420:
```javascript
const ACCESS_PASSCODE = 'TTTG2026';
```
Change `'TTTG2026'` to your new passcode and push to GitHub.

---

*Maintained for Top Tier Talent Group & JA Studio Consulting.*
