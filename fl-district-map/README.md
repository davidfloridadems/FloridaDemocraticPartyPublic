# Florida District Overlap Map

An interactive map of Florida's congressional, state senate, state house and county boundaries on one map, built for planning events and outreach across districts.

- **Search an address** or **click the map** to see every district that covers a spot.
- **Search a district or county** (e.g. `HD 65`, `SD 20`, `Broward`) to see every district it overlaps and the share of residents they have in common.
- **Share a link** to a specific view: the URL updates as you go.
  - `#cd-14`, `#sd-20`, `#hd-65`: a district
  - `#county-miami-dade`: a county
  - `#pt=27.95,-82.46`: a location

It's a static site with no build step and no server.

## Publish on GitHub Pages

1. Create a public repository (or use an existing one) and upload the contents of this folder. Keep `index.html`, `data/`, and `vendor/` together.
2. In the repo, go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)` (or the folder you put it in), then **Save**.
3. After a minute or two the map is live at `https://<org>.github.io/<repo>/`. If you put it in a subfolder, add the folder name to the end.

To embed it on another site, use an iframe:

```html
<iframe src="https://<org>.github.io/<repo>/" style="width:100%;height:720px;border:0" title="Florida District Overlap Map"></iframe>
```

## Data sources

| Layer | Source | Notes |
|---|---|---|
| Congressional districts | 2026 enacted plan (HB 1-D) block assignment file | Built by merging 2020 Census blocks by district, then clipped to the shoreline |
| State Senate and State House | 2022 plans. Districts come from U.S. Census Bureau 2023 boundaries (`cb_2023_12_sldu_500k`, `cb_2023_12_sldl_500k`), assigned to 2020 blocks, then rebuilt from those blocks | Rebuilding from blocks lets all four layers share exactly the same edges |
| Counties | 2020 Census blocks merged by county | All layers are clipped to the same shoreline |
| Population and overlaps | 2020 Census block population (`POP20`) | Each block is assigned to districts by an interior point |

Overlap percentages are *share of the selected district's residents*. Overlaps smaller than about 0.2% of a district, or under 100 people, are hidden. These are usually artifacts of the simplified boundary lines, not real overlaps.

The boundaries are simplified for fast loading, so they are accurate for planning but not parcel-level. For official district lookups, use your county Supervisor of Elections.

### Data files

- `data/fl-districts.js`: everything the page loads (boundaries, labels, overlaps, county names)
- `data/fl-districts.topojson`: boundaries only (layers `cd`, `sd`, `hd`, `county`; property `d` = district number or county FIPS)
- `data/overlap.json`: population overlaps, `layer → id → {pop, <otherLayer>: [[id, people], …]}`

## Third-party services

- Basemap tiles: Esri Light/Dark Gray Canvas (no API key; attribution © Esri, HERE, Garmin, OpenStreetMap contributors). If those tiles fail to load, the page falls back to standard [OpenStreetMap](https://www.openstreetmap.org/copyright) tiles. Keep the attribution visible.
- Address search uses [OpenStreetMap Nominatim](https://operations.osmfoundation.org/policies/nominatim/). It's free for light use (at most 1 request per second, which the page enforces). If the map gets heavy traffic, switch to a keyed geocoder.
- [Leaflet](https://leafletjs.com) and [topojson-client](https://github.com/topojson/topojson-client) are bundled in `vendor/` (BSD-2 / ISC licenses).
