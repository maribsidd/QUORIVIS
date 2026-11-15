# Quorivis

Two decades of India's fires, on one consistent scale.

Satellites have recorded active fires since 2002, but the older MODIS sensor and the newer VIIRS sensors count fires differently, so their records cannot be compared directly. Quorivis merges them into a single monthly series for any area, shows how reliable the merge is, and compares recent months with history.


## What it does

- Pick a region of India or draw your own area on the map.
- See a harmonized monthly series from 2002 to 2025 with a 90% range.
- Read the burning calendar (year by month) and the last 12 months against the historical median.
- See how the merge performs on years it was not calibrated on.
- Download the monthly series for your area as CSV.

## Run it

Link: https://maribsidd.github.io/QUORIVIS/

or

Open `index.html` in a browser. The data is built into the page, so nothing needs to be installed or uploaded.

## How it works

Detections are placed on a 0.01 degree grid and counted as cell-days, which removes the resolution advantage of VIIRS. For each area and season, VIIRS is scaled to MODIS units using calibration years, and the scaled series continues the MODIS record. See [METHOD.md](METHOD.md) for the full method, validation results and limits.

## Rebuild the data

`build_tiles.py` converts FIRMS Archive Download CSVs (MODIS C6.1, VIIRS S-NPP, VIIRS NOAA-20) into the compact tile data embedded in the page.

```
python build_tiles.py --modis M.csv --snpp S.csv --n20 J.csv --out tiles.json
```

Requires Python 3 with pandas and numpy.

## Data

We acknowledge the use of data from NASA's Fire Information for Resource Management System (FIRMS, https://earthdata.nasa.gov/firms), part of NASA's Land, Atmosphere Near real-time Capability for EOS (LANCE) and Earth Science Data and Information System (ESDIS). MODIS Collection 6.1 and VIIRS 375 m active fire detections, India, 2002 to 2025, standard processing.
