# Data Cleaning & Integration Notes

## Merge Strategy: Inner Join vs Outer Join
We tested two join strategies when merging air quality and climate data on
`location_name` + `date`:

| Join type | Rows | PM2.5 completeness |
|---|---|---|
| Outer/left join | 125,994 | 5.3% |
| **Inner join (selected)** | **7,155** | **93.1%** |

The outer join preserved a much larger row count, but retained large numbers of
weather-only records with no corresponding air quality reading (94.7% missing on
PM2.5). We selected the inner join, prioritizing analytical validity - a smaller
dataset where nearly every row is usable, over a larger but mostly incomplete one.

## Columns Removed and Why
- **`o3` (ozone):** Dropped. Values were invalid - mean of -0.44 ppm with a range of
  -0.999 to 0.076, which is physically impossible (ozone cannot be negative). This
  points to a sensor calibration or unit-conversion issue at the source, not a
  correctable outlier.
- **`pm10`:** Dropped. 93.3% missing - reported by too few stations to be usable.
- **`relativehumidity`, `temperature` (from air quality sensors):** Dropped.
  ~42–44% missing and redundant with equivalent, more complete columns already
  sourced from NASA POWER (`humidity`, `temperature`).
- **`um003` (particle count):** Dropped. 44.2% missing, not part of the project's
  core pollutant metrics.

## Columns Retained
- `pm25` - primary pollutant metric, 93.1% complete (6.9% missing, explained by
  station sensor type - not all stations measure PM2.5).
- `pm1` - kept as an optional secondary metric from low-cost sensor stations only,
  55.8% missing. Used selectively, not in core charts.

## Validation Checks Performed
- **Duplicates:** 0 exact duplicate rows; 0 duplicate station+date combinations.
- **Negative values:** 0 negative readings across temperature, rainfall, humidity,
  PM2.5, and PM1 after dropping the invalid O3 column.
- **Humidity range:** 0 values outside the valid 0-100% range.
- **Precipitation sanity check:** Monthly averages follow Nepal's known monsoon
  pattern (July 11.7mm and August 9.6mm as wettest; December 0.04mm and January
  0.04mm as driest), confirming the climate data reflects real seasonal patterns
  rather than sensor artifacts.
- **Extreme values:** PM2.5 maximum of 438 µg/m³ was checked against documented
  real-world Kathmandu winter pollution episodes and retained as plausible rather
  than treated as an error.

## Known Limitations
- PM2.5 has 6.9% missing values, concentrated at stations without reference-grade
  monitors.
- PM1 has significant missingness (55.8%) and is only usable for secondary,
  low-cost-sensor-specific analysis.
- Tourism data is national-level only and joined on year+month, not station -
  it cannot support district-level economic claims.
