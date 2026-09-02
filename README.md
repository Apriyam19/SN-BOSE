# SN-BOSE — Majuli Flood & Erosion Risk Analysis

## Project Description
A **Google Earth Engine (GEE)** script that maps flood-affected areas on **Majuli Island, Assam** by comparing Sentinel-1 SAR radar imagery from before and after the 2023 monsoon flood. It detects new flood water, visualizes the change on the map, and computes the total flooded area (in sq. m). It also exports the study-area boundary shapefiles to Google Drive.

> 
## Tech Stack
| Component | Details |
|---|---|
| Platform | Google Earth Engine (GEE) Code Editor |
| Language | Earth Engine JavaScript API |
| Satellite Data | Sentinel-1 SAR — `COPERNICUS/S1_GRD` (VV polarization, IW mode, descending orbit) |
| Export | Google Drive (Shapefile format via `Export.table.toDrive`) |
| Input | Pre-uploaded GEE table assets (`table`, `table2`, `table3`, `table4`) representing Majuli boundary/shapes |

## Project Structure
```
SN-BOSE-main/
└── Majuli Flood Analysis     # Single GEE script (no file extension)
```
- No build files, package manager configs, or additional modules — this is a single standalone script meant to be pasted into the GEE Code Editor.

## Setup & Installation
No local installation is required since this runs entirely in the browser-based GEE Code Editor.

1. **Create/log in** to a Google Earth Engine account: https://code.earthengine.google.com
2. **Upload the required assets** to your GEE account as Table assets, and rename them (or update the variable names in the script) so they map to:
   - `table` → shape1
   - `table2` → shape2 (used as the Majuli study area boundary)
   - `table3` → shape3
   - `table4` → shape4
3. **Open a new script** in the Code Editor and paste the contents of `Majuli Flood Analysis` into it.

## Running the Project
1. Click **Run** in the GEE Code Editor.
2. The map will center on Majuli and display:
   - Majuli boundary (red outline)
   - "Before Flood" SAR image (May 2023)
   - "After Flood" SAR image (Aug–Sep 2023)
   - "SAR Difference" (change detection layer)
   - "Flood Prone Areas" (blue mask of newly flooded pixels)
3. Check the **Console tab** for the printed `Flood Area (sq.m)` statistic.
4. Go to the **Tasks tab** and manually run the export tasks to send the shapefiles to your Google Drive (`gee_exports` folder).

## Limitations / Troubleshooting
- **Undefined variables**: `table`, `table2`, `table3`, `table4` are GEE asset placeholders — the script will fail with a `table is not defined` error unless these assets are imported into the script's *Imports* section first.
- **Incomplete feature set**: NDVI, rainfall, and slope-based erosion analysis are referenced in comments but not implemented — only flood detection via SAR difference is functional.
- **Threshold sensitivity**: the flood mask uses a fixed SAR difference threshold (`> 3`), which may need tuning for other regions/time periods — false positives can occur from noise, boats, or wind-roughened water.
- **Fixed date ranges**: "before" and "after" flood windows are hardcoded to 2023; update the `.filterDate()` calls to analyze other years/events.
- **Export step is manual**: `Export.table.toDrive` tasks are only queued, not auto-run — they must be started manually from the Tasks tab.
- **Scale/pixel limits**: `reduceRegion` uses `scale: 10` and `maxPixels: 1e13`; very large study areas may hit computation timeouts and require tiling or a coarser scale.
