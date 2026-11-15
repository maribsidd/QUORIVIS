# Method

## Purpose

Produce one consistent monthly record of fire activity from MODIS (2002 onward) and VIIRS (2012 onward) for any area, so that the sensor of origin does not matter.

## Data

- MODIS Collection 6.1, Aqua, 2002-07 to 2025-12.
- VIIRS 375 m, Suomi NPP, 2012-01 to 2025-12.
- VIIRS 375 m, NOAA-20, 2018-04 to 2025-12.
- Source: NASA FIRMS Archive Download, India, standard processing. About 14 million detections in total.

Terra is not used: its overpass time differs from Aqua's and VIIRS's. Fire radiative power is not used: VIIRS saturates over intense fires.

## Cleaning

1. Drop low-confidence detections (MODIS confidence below 30, VIIRS class `l`).
2. Keep vegetation-fire types only (`type` 0).
3. Mask cells with detections in at least half of all months of a sensor's record. This removes persistent heat sources such as industry. It masked 529 cells for Suomi NPP, 568 for NOAA-20 and none for Aqua.

## Unit

Each detection is placed on a 0.01 degree grid (about 1 km). A cell with at least one detection on a given day is one cell-day. Counting cell-days instead of pixels removes the advantage of VIIRS's finer 375 m resolution. Counts are summed to 0.5 degree tiles per month, and an area is the sum of its tiles.

## Harmonization

For a chosen area, let A be Aqua cell-days and V be Suomi NPP cell-days per month. For each season s (DJF, MAM, JJA, SON) and the calibration years:

```
k_s = sum(A) / sum(V)
```

From the first calibration year on, the harmonized series is `k_s * V`. Before it, the series is Aqua as measured. Units are MODIS-equivalent cell-days.

Where both VIIRS satellites overlap (2018 to 2025), Suomi NPP counts are 0.97 of NOAA-20's, so the two agree closely. NOAA-20 would extend the series if Suomi NPP data ended before the record does; at present Suomi NPP covers the whole period.

## Uncertainty

The calibration years are resampled with replacement 300 times and the factors recomputed. The 5th and 95th percentiles of the factors give the 90% range on every VIIRS-based month.

## Validation

Each calibration year is held out in turn: factors are fitted on the other years and used to predict Aqua's counts for the held-out year from VIIRS. The app reports the median absolute error and the Spearman rank correlation. After the calibration years, it also reports Aqua divided by calibrated VIIRS for each year.

Results with calibration years 2013 to 2020:

| Area | Median error | Spearman |
|---|---|---|
| All India | 15% | 0.98 |
| Odisha and Chhattisgarh | 20% | 0.99 |
| Northeast India | 20% | 0.99 |
| Punjab and Haryana | 28% | 0.96 |

In Punjab and Haryana, Aqua divided by calibrated VIIRS rises from 1.06 in 2021 to 1.55 in 2025, while for India as a whole it stays near 1.00 until 2025. This points to the older MODIS sensor changing over time, for example its overpass time drifting, and is the reason the series continues with VIIRS after calibration.

## Limits

- Results are in MODIS-equivalent units, not absolute fire counts.
- Day and night detections are combined.
- Each area gets one scale factor per season.
- Tiles are 0.5 degrees, so area outlines are approximate.
- The current month is compared with earlier years of the same calendar month; it is not a forecast.

## References

- Giglio, L., Schroeder, W., & Justice, C. O. (2016). The collection 6 MODIS active fire detection algorithm and fire products. Remote Sensing of Environment, 178, 31-41.
- Schroeder, W., Oliva, P., Giglio, L., & Csiszar, I. A. (2014). The new VIIRS 375 m active fire detection data product. Remote Sensing of Environment, 143, 85-96.
