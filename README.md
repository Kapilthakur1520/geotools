# GeoTools — IAC & UTM Grid Viewer

A lightweight, browser-based map viewer that overlays IAC grid and UTM zone information on a Leaflet map. It includes search (Nominatim), multiple base layers, drawing/analysis tools, and a real-time status bar for coordinates, IAC sheet, and UTM zone.

## Features

- Interactive Leaflet map with custom IAC grid overlay and labels.
- Dynamic label detail based on zoom level (million/degree/topo/quad).
- UTM zone calculation (zone number + hemisphere).
- Real-time status bar showing:
  - IAC Sheet
  - UTM Zone
  - Mouse Lat/Lon
- Search using Nominatim (OpenStreetMap) to pan the map to places.
- Multiple tile basemaps (Carto Light, OSM, Carto Dark).
- Basic drawing/analysis tools (point, rectangle, polygon) via Leaflet.draw.
- Simple UI with translucent controls and map label styling.

## Quick start

Requirements:
- Modern web browser (Chrome, Firefox, Edge, Safari).
- Internet connection (the page loads Leaflet, Leaflet.draw and tile layers from CDNs).

To run locally:
1. Clone or download this repository.
2. Open `index.html` in your browser:
   - Double-click the file or
   - Serve it with a simple static server (recommended to avoid certain browser restrictions):
     - Python 3: `python -m http.server 8000` then open `http://localhost:8000/`
     - Node (http-server): `npx http-server` then open the provided URL

Notes:
- The app relies on external CDNs for Leaflet and Leaflet.draw and uses Nominatim for geocoding — ensure your environment allows outbound HTTP(S) requests.
- Nominatim enforces rate limits and usage policies; for production or heavy use consider using a geocoding service with an API key or hosting your own instance of Nominatim.

## How it works (implementation notes)

- The grid and labels are generated in `index.html` script:
  - `zones` array contains the IAC sheet definitions and is used by `getIACData(lat, lng)` to compute the sheet components (million, degree, topo, quad).
  - `getUTMString(lat, lng)` computes the UTM zone number and hemisphere (N/S).
  - `updateMapGrid()` draws rectangles and `L.divIcon` labels across the current map bounds, with label detail varying by zoom.
- Interaction:
  - Mouse move updates the status bar (lat/lon, IAC, UTM).
  - Click places a marker with a popup showing the sheet and coordinates.
  - Search box queries Nominatim and recenters the map.
  - Tile selector switches between base layers.
  - Draw tools are provided via Leaflet.draw, and drawn features are stored in a FeatureGroup.



## Development

- Main file: `index.html` (single-file front-end).
- Libraries:
  - Leaflet 1.7.1 (CDN)
  - Leaflet.draw 1.0.4 (CDN)
  - Carto / OSM tile layers (used via public tile endpoints)
- Commit snapshot (reference): commit OID 7f800a82810261273dd36573a6cf7359f82dfddc (source used to create this README).

If you plan to extend the project, consider splitting JavaScript into modules, adding build tooling, and writing unit tests for spatial computations.

