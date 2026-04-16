# Project Data Schema

This file defines the target structure that all datasets in this project
must conform to after cleaning. Every cleaning notebook should validate
its output against this schema before saving to `data/processed/`.

---

## Core requirements

| Property | Value |
|---|---|
| File format | `.parquet` |
| Timestamp column | `timestamp` — always present, always UTC |
| Granularity | Hourly (1 row per station per hour) |
| City coverage | Belo Horizonte, Brasília, Manaus, Porto Alegre, São Paulo |
| Time range | 2022-01-01 → 2025-12-31 |

---

## Required columns (all datasets must have these)

| Column | Type | Description | Example |
|---|---|---|---|
| `timestamp` | `datetime64[us, UTC]` | UTC datetime, hourly | `2022-01-01 00:00:00+00:00` |
| `city` | `str` | Standardized city name | `Belo Horizonte` |
| `year` | `int64` | Calendar year | `2022` |

---

## INMET weather columns

| Column | Type | Nulls allowed | Description |
|---|---|---|---|
| `precip_mm` | `float64` | <1% | Hourly precipitation (mm) |
| `pressure_mb` | `float64` | <1% | Atmospheric pressure (mB) |
| `pressure_max_mb` | `float64` | <1% | Max pressure previous hour (mB) |
| `pressure_min_mb` | `float64` | <1% | Min pressure previous hour (mB) |
| `radiation_kjm2` | `float64` | <25% | Global radiation (kJ/m²) — null at night |
| `temp_c` | `float64` | <1% | Dry bulb temperature (°C) |
| `dewpoint_c` | `float64` | <1% | Dew point temperature (°C) |
| `temp_max_c` | `float64` | <1% | Max temperature previous hour (°C) |
| `temp_min_c` | `float64` | <1% | Min temperature previous hour (°C) |
| `dewpoint_max_c` | `float64` | <1% | Max dew point previous hour (°C) |
| `dewpoint_min_c` | `float64` | <1% | Min dew point previous hour (°C) |
| `humidity_max_pct` | `float64` | <1% | Max relative humidity previous hour (%) |
| `humidity_min_pct` | `float64` | <1% | Min relative humidity previous hour (%) |
| `humidity_pct` | `float64` | <1% | Relative humidity (%) |
| `wind_dir_deg` | `float64` | <2% | Wind direction (degrees) |
| `wind_gust_ms` | `float64` | <2% | Max wind gust (m/s) |
| `wind_speed_ms` | `float64` | <2% | Wind speed (m/s) |

---

## Station metadata columns (INMET only)

| Column | Type | Description | Example |
|---|---|---|---|
| `estacao` | `str` | Station name | `BELO HORIZONTE - PAMPULHA` |
| `codigo_wmo` | `str` | WMO station code | `A521` |
| `uf` | `str` | Brazilian state code | `MG` |
| `latitude` | `float64` | Station latitude | `-19.883889` |
| `longitude` | `float64` | Station longitude | `-43.969444` |

---

## Standardized city names

All datasets must use exactly these city name strings — no variations:

- `Belo Horizonte`
- `Brasília`
- `Manaus`
- `Porto Alegre`
- `São Paulo`

---

## Null policy

- Short gaps (≤2 consecutive hours): interpolated linearly during cleaning
- Long gaps (>2 hours): left as `NaN` — never fabricated
- `radiation_kjm2`: high null rate (~24%) is expected and legitimate — nighttime readings
- Stations with >50% null rate for a given year: excluded with documented justification

---

## Validation checklist

Every cleaning notebook must confirm before saving:

- [ ] `timestamp` is `datetime64[us, UTC]` with no nulls
- [ ] `city` values match exactly the 5 standardized names above
- [ ] `year` column present as `int64`
- [ ] No duplicate `timestamp` + `city` + `station` combinations
- [ ] Parquet saved to `data/processed/` with `index=False`

---

## Processed files

| File | Dataset | Rows | Last updated |
|---|---|---|---|
| `inmet_clean.parquet` | INMET weather | 266,592 | 2026-04 |
| `sisam_clean.parquet` | SISAM air quality | TBD | — |
| `queimadas_clean.parquet` | INPE fire hotspots | TBD | — |
| `integrated.parquet` | All datasets joined | TBD | — |
