# Global Humanitarian Needs Tracker

An interactive web map that visualizes where humanitarian aid is most needed worldwide. Country-level risk is shaded using the INFORM Risk Index, and active emergencies are plotted as proportional symbols sized by people in need.

https://melaku3x.github.io/humanitarian-map/

Built with vanilla HTML, CSS, and JavaScript using [Leaflet](https://leafletjs.com/). No build step, no framework, no dependencies beyond a single CDN link.

## Live demo

Open `index.html` in a browser, or visit the deployed version on GitHub Pages.

## Overview

The map combines two cartographic techniques on a single base map.

A **choropleth layer** fills every country with a color from a seven-step sequential scale (cream to dark red) reflecting its score on the INFORM Risk Index — a composite indicator published by the European Commission's Joint Research Centre that combines hazard exposure, vulnerability, and lack of coping capacity into a single 0–10 value.

A **proportional symbol layer** plots currently active humanitarian emergencies (Sudan, the Democratic Republic of the Congo, Afghanistan, Ethiopia, Myanmar, Yemen, Syria, Ukraine, Somalia, the Sahel, Haiti, Lebanon, Gaza, Mozambique, South Sudan, and others) as red circles centered on the affected region. Circle area is proportional to the number of people in need so that the visual encoding matches how human perception reads size. Clicking a circle opens a popup with crisis-level details: people in need, percentage of the UN appeal funded, and main drivers.

A sidebar surfaces three global totals — people in need, UN appeal funding gap, and forcibly displaced population — along with toggles for each map layer. A legend in the lower-right explains the color scale.

## Features

- Interactive Leaflet map with country-level choropleth and crisis-point overlay
- Hover tooltips on every country showing its INFORM Risk score
- Click-to-open popups on each crisis marker with PIN, funding, and drivers
- Independent layer toggles for the choropleth and the marker layer
- Responsive layout that works on desktop and tablet
- Single self-contained HTML file with no build pipeline

## Tech stack

- **Leaflet 1.9** — open-source JavaScript library for interactive maps
- **CARTO Light basemap** — neutral grey tile layer chosen so that thematic colors stand out
- **GeoJSON** — public country-boundary polygons from `johan/world.geo.json`, joined to the risk table on ISO 3166-1 alpha-3 country codes
- **Vanilla CSS** for layout and styling

## Data sources

Figures in this build are illustrative and approximate, drawn from the publicly available sources below. For a production version they would be wired to live feeds.

- INFORM Risk Index — European Commission Joint Research Centre: <https://drmkc.jrc.ec.europa.eu/inform-index/>
- UN OCHA Global Humanitarian Overview 2025
- UN OCHA Humanitarian Data Exchange: <https://data.humdata.org/>
- UNHCR Refugee Data Finder: <https://www.unhcr.org/refugee-statistics/>
- ACAPS crisis severity assessments: <https://www.acaps.org/>

## How it works

The application is structured in seven steps inside `index.html`, each marked with a numbered comment in the source.

The Leaflet map is initialized and a CARTO tile layer is added as the basemap. An INFORM Risk lookup table is defined, keyed by ISO3 country code, and paired with a sequential color scale that maps risk scores to fill colors. The world country GeoJSON is then fetched; each polygon feature is styled by joining to the risk table on its ISO3 code, and hover and tooltip handlers are attached. The active-crisis array is iterated over, drawing a `circleMarker` for each entry. Radius is set to the square root of PIN so that circle area scales linearly with people in need — the perceptually correct way to size proportional symbols. A custom Leaflet control renders the legend, and the sidebar checkboxes are wired up to add and remove each layer from the map.

The attribute join between the country polygons and the risk table — using the ISO3 code as the shared key — is the central GIS operation in the project. The same pattern generalizes to any country-level indicator.

## Roadmap

Possible extensions for future iterations:

- Pull live data from the UN OCHA Humanitarian Data Exchange on a schedule instead of hard-coding values
- Add a time slider showing how PIN has evolved year over year
- Add sub-national administrative boundaries for one or two countries to demonstrate multi-scale analysis
- Migrate from Leaflet raster tiles to MapLibre GL vector tiles for smoother styling and 3D terrain
- Provide a companion QGIS project file reproducing the analysis on the desktop

## License

Code released under the MIT License. Data sources retain their respective licenses; see links above for terms.
