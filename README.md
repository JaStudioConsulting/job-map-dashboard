# 🗺️ Active Jobs Map & Candidate Distance Ranking Dashboard

An interactive single-page dashboard built for **Top Tier Talent Group** to visualize active job openings across North America, query candidate locations, and automatically rank open roles by driving/straight-line distance in real-time.

---

## ✨ Features

- **Live Google Sheets Synchronization:** Connects directly to the live Google Sheets CSV endpoint to parse and filter all `Status === 'Active'` jobs in real-time.
- **Instant Radius & Distance Ranking:** Type any candidate's city, state/province, or postal code to calculate exact distances (in **km** or **miles**) using the Haversine formula and rank roles closest-to-farthest.
- **Interactive Leaflet.js Map:** OpenStreetMap tiles with custom styled pins, hover tooltips, and fly-to focus animations.
- **Smart Geocoding Engine:** Built-in high-speed coordinate dictionary for key Ontario & North American recruitment hubs, with an automatic fallback to the OpenStreetMap Nominatim geocoding API.
- **Candidate Pitch Drawer & Quick Copy:** Click any pin or card to inspect full role details, compensation, client contact, owners, and copy a formatted summary with 1 click.
- **Quick Recruiter Filters:** Keyword search (title/skill/company), Owner filter dropdown, and dynamic Max Radius sliders (25km, 50km, 100km, 250km).
- **Zero-Build Deployment:** Standalone production SPA that works right out of the box with GitHub Pages.

---

## 🚀 Quick Start & Local Preview

You can open `index.html` directly in any web browser:

```bash
# Open directly on macOS
open index.html

# Or serve locally with any static server
npx serve .
# or
python3 -m http.server 8000
```

---

## 📦 How to Push to GitHub (`jastudioconsulting`)

To publish this repository under your **`jastudioconsulting`** organization on GitHub:

```bash
cd /Users/TTTG/Documents/Mother_Work/job-map-dashboard

# Initialize git repository
git init
git add .
git commit -m "feat: initial release of interactive job map and distance ranking dashboard"

# Create repo on GitHub under jastudioconsulting and link it:
# (Replace repo-name with your preferred repository name, e.g. job-map-dashboard)
git branch -M main
git remote add origin https://github.com/jastudioconsulting/job-map-dashboard.git
git push -u origin main
```

### 🌐 Enabling 1-Click Free Hosting (GitHub Pages):
1. Go to your repository on GitHub: `https://github.com/jastudioconsulting/job-map-dashboard/settings/pages`
2. Under **Build and deployment** $\rightarrow$ **Source**, choose **Deploy from a branch**.
3. Select branch **`main`** / folder **`/ (root)`** and click **Save**.
4. Your live dashboard will be instantly available at:
   `https://jastudioconsulting.github.io/job-map-dashboard/`

---

## ⚙️ Configuration & Live Endpoint

The dashboard pulls live data from:
```
https://docs.google.com/spreadsheets/d/1RexJFAEWRwNjzeGThutrA0zMObu2g4r3q0Vu0uiDbQs/gviz/tq?tqx=out:csv&sheet=Jobs
```

If the spreadsheet permissions or sheet tab name change, you can update the `CSV_URL` constant inside `index.html`.
