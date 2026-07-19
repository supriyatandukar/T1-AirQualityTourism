# Data Sources

## 1. Air Quality - OpenAQ
- **Source:** OpenAQ API v3 (https://docs.openaq.org)
- **Provider notes:** Aggregates government reference monitors (e.g. AirNow/Embassy
  Kathmandu) and low-cost sensor networks across Nepal
- **Parameters used:** PM2.5 (µg/m³), PM1 (µg/m³, secondary metric)
- **Coverage:** 62 stations across Nepal (after merge with weather data)
- **Time range:** Daily readings, 2021-01-01 to 2025–12–31
- **License:** Varies by station provider; government stations under public domain,
  low-cost sensor data per individual station license (see OpenAQ location metadata)

## 2. Climate Data - NASA POWER
- **Source:** NASA Prediction of Worldwide Energy Resources (POWER) API
  (https://power.larc.nasa.gov)
- **Parameters used:**
  - T2M - Temperature at 2 meters (°C)
  - PRECTOTCORR - Corrected total precipitation (mm/day)
  - RH2M - Relative humidity at 2 meters (%)
- **Coverage:** Pulled for the same coordinates as air quality stations (deduplicated
  by rounded lat/lon), daily, 2021–2025
- **Note:** Used in place of NOAA/ICIMOD (suggested in module brief) as a freely
  accessible, no-authentication alternative providing equivalent daily climate variables
  for Nepal.

## 3. Tourist Arrivals - Nepal Tourism Board
- **Source:** *Nepal Tourism Statistics 2025*, Ministry of Culture, Tourism &
  Civil Aviation, Government of Nepal
- **Table used:** Table 2.2 - Tourist Arrival by Month, 1995–2025
- **Coverage:** National-level monthly tourist arrivals, filtered to 2021–2025
  for this project
- **Extraction method:** Manually transcribed from official PDF report (Department
  of Immigration, Nepal, as cited in the source document)
- **Limitation:** This data is national-level only; it cannot be broken down by
  district or station, unlike the air quality and climate datasets.

## Integration Notes
Air quality and climate data are joined on station name + date (inner join).
Tourism data is joined separately on year + month only, since it has no
station-level granularity. See `data_cleaning.md` for full merge reasoning.
