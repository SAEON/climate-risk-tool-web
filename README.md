# Climate Risk Tool - Web Application

React-based climate risk mapping tool for South African municipalities with Leaflet visualization.

## Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Lint code
npm run lint

# Fix linting issues
npm run lint:fix
```

## Prerequisites

- Node.js v22.21.0+
- Backend API running on `http://localhost:3000` (requires VPN for database access)
- PostgreSQL 17.6 + PostGIS 3.6 (remote at 192.168.115.78:5432)

## Project Structure

```
climate-risk-tool-web/
├── public/                             # Static assets
│   ├── favicon.ico                     # Browser favicon
│   ├── favicon.svg                     # SVG favicon
│   ├── favicon-96x96.png               # PNG favicon
│   ├── apple-touch-icon.png            # iOS home screen icon
│   ├── web-app-manifest-192x192.png    # PWA icon (192x192)
│   ├── web-app-manifest-512x512.png    # PWA icon (512x512)
│   ├── saeon_logo.png                  # SAEON/NRF logo (PNG)
│   ├── saeon.svg                       # SAEON/NRF logo (SVG)
│   └── site.webmanifest                # PWA manifest
├── docs/                               # Documentation
│   ├── COLOR_SCHEME_GUIDE.md           # Color scheme documentation
│   └── Downscaled Climate Change Projections for SA v1.0.pdf
├── src/
│   ├── api/                            # API service layer
│   │   ├── client.js                   # Axios instance
│   │   ├── climateData.js              # Climate data endpoints
│   │   ├── indices.js                  # Climate indices endpoints
│   │   └── municipalities.js           # Municipality endpoints
│   ├── components/
│   │   ├── Common/                     # Shared components
│   │   │   ├── DataAttribution.jsx     # Data source attribution
│   │   │   └── SectorTags.jsx          # Sector tags display
│   │   ├── Map/                        # Map components
│   │   │   ├── Map.jsx                 # Base Leaflet map
│   │   │   ├── ClimateLayer.jsx        # GeoJSON climate layer
│   │   │   └── Map.module.css
│   │   ├── Controls/                   # UI controls
│   │   │   ├── ScenarioSelector.jsx
│   │   │   ├── ComparisonScenarioSelector.jsx
│   │   │   ├── PeriodSelector.jsx
│   │   │   ├── IndexSelector.jsx
│   │   │   ├── MunicipalitySearch.jsx
│   │   │   └── ComparisonToggle.jsx
│   │   ├── Legend/                     # Legend component
│   │   │   └── Legend.jsx
│   │   ├── InfoPanel/                  # Municipality info
│   │   │   └── InfoPanel.jsx
│   │   ├── Compare/                    # Comparison view
│   │   │   └── ComparisonView.jsx
│   │   └── Layout/                     # Layout components
│   ├── context/                        # React contexts
│   │   ├── ClimateContext.jsx          # Global climate state
│   │   └── IndicesContext.jsx          # Climate indices metadata
│   ├── utils/                          # Utility functions
│   │   ├── constants.js                # Configuration & constants
│   │   └── colorMapping.js             # Color scales & data processing
│   ├── App.jsx                         # Main application
│   ├── main.jsx                        # React entry point
│   └── index.css                       # Global styles
├── index.html                          # HTML entry point
├── .env                                # Environment variables
├── .gitignore                          # Git ignore rules
├── LICENSE                             # MIT License
├── vite.config.js                      # Vite configuration
├── tailwind.config.js                  # Tailwind CSS configuration
├── postcss.config.js                   # PostCSS configuration
├── package.json                        # Dependencies & scripts
└── package-lock.json                   # Dependency lock file
```

## Features

### Core Features
- Interactive Leaflet map centered on South Africa
- Dynamic GeoJSON rendering for 213 municipalities
- Color-coded climate anomaly visualization
- Scenario selection (SSP1-2.6, SSP2-4.5, SSP3-7.0, SSP5-8.5)
- Time period selection (2021-2040, 2041-2060, 2081-2100)
- Climate index selection (27 indices across 3 categories)
- Municipality details on click
- Dynamic legend with statistics

### Advanced Features
- **Side-by-side scenario comparison** for scenario analysis
- **Municipality search** with auto-zoom functionality
- Synchronized period and index across comparison views
- Category-based index filtering (Precipitation, Temperature, Duration)

### Data Integration
- Leverages full API response metadata
- Auto-fetch on configuration changes
- Cache header tracking (X-Cache: HIT/MISS)
- Proper handling of climate anomalies (deviations from 1995-2014 baseline)

## Data Coverage

- **Municipalities**: 213 (South African local municipalities)
- **Scenarios**: 4 SSP scenarios
- **Time Periods**: 3 periods (near/mid/far-term)
- **Climate Indices**: 27 indices
  - 12 Precipitation indices
  - 12 Temperature indices
  - 3 Duration/Variability indices
- **Total Records**: 2,544 climate data points

## API Endpoints Used

```javascript
// Climate Data (GeoJSON)
GET /api/climate-data/geojson/{scenario}/{period}/{index}

// Climate Indices Metadata
GET /api/indices
GET /api/indices/category/{category}

// Municipalities
GET /api/municipalities

// Scenarios & Periods
GET /api/climate-data/scenarios
GET /api/climate-data/periods

// Cache Management
GET /api/cache/stats
POST /api/cache/clear
```

## Color Mapping

Uses Chroma.js with scientifically-appropriate color schemes:
- **RdBu_r**: Red-Blue reversed (Red = worse)
- **BuRd**: Blue-Red (Blue = better)
- **RdBu**: Red-Blue (standard diverging)

Anomaly directions:
- `positive_bad`: Positive = worse (e.g., drought, heat)
- `positive_good`: Positive = better (e.g., more rain)
- `negative_warming`: Negative = warming
- `positive_warming`: Positive = warming

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

## Environment Variables

```env
# API Configuration
VITE_API_BASE_URL=/climate-tool/api

# Map Configuration
VITE_MAP_CENTER_LAT=-28.75
VITE_MAP_CENTER_LNG=22.9
VITE_MAP_ZOOM=4.7
VITE_MAP_MIN_ZOOM=5
VITE_MAP_MAX_ZOOM=12
```

## Tech Stack

- **Frontend**: React 18.3.1
- **Build Tool**: Vite 7.1.12
- **Mapping**: Leaflet 1.9.4 + React Leaflet 4.2.1
- **Styling**: Tailwind CSS 3.4.4
- **Color Scales**: Chroma.js 3.1.1
- **HTTP Client**: Axios 1.7.2
- **Export**: html2canvas + jsPDF (for future export feature)

## Future Enhancements

- [ ] Map export to PNG/PDF
- [ ] Provincial-level aggregation
- [ ] Advanced filtering (by province)
- [ ] Data download (CSV, JSON)
- [ ] Mobile-optimized layout
- [ ] Accessibility improvements (ARIA labels, keyboard navigation)

## License

MIT

## Author

Climate Risk Tool Development Team
