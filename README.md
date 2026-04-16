# Air Quality & Urban Health in Brazilian Cities

A full data science portfolio project analyzing weather patterns and air quality
across five Brazilian cities (Belo Horizonte, Brasília, Manaus, Porto Alegre,
São Paulo) from 2022 to 2025.

## Project structure
air_quality_brazil/
├── data/
│   ├── raw/          # original source files (not tracked by Git)
│   └── processed/    # cleaned parquet files (not tracked by Git)
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_cleaning_merging.ipynb
│   └── 03_eda.ipynb
├── SCHEMA.md         # target data schema for all datasets
├── pyproject.toml    # project dependencies
└── README.md

## Data sources

- **INMET** — hourly weather data from automatic stations (bdmep.inmet.gov.br)
- **SISAM/CPTEC** — air quality monitoring (PM2.5, PM10, CO, O3) *(in progress)*
- **INPE Queimadas** — fire hotspot data *(in progress)*

## Pipeline

1. **Data collection** — bulk download from government APIs
2. **Cleaning & merging** — standardized schema, UTC timestamps, interpolation
3. **EDA** — seasonal patterns, correlations, anomaly detection
4. **Modeling** — AQI prediction (linear regression → Random Forest), city clustering (K-Means) *(planned)*
5. **Dashboard** — Power BI interactive dashboard *(planned)*

## Key findings so far

- 2024 was an anomalous year across multiple variables simultaneously — severe drought
  in the southeast/center-west and catastrophic flooding in Porto Alegre (May 2024,
  Z-score = +4.17) — consistent with strong El Niño
- Manaus shows a unique atmospheric feedback loop: dry season heat drives
  evapotranspiration → cloud buildup → radiation drop → wet season onset
- Five distinct climate regimes identified, each requiring separate treatment in modeling

## This project is a documented learning journey

This portfolio intentionally shows progression — including the ML components being
built incrementally as new concepts are learned. The analytical reasoning behind
each decision is documented in notebook markdown cells.

## Setup

```bash
git clone https://github.com/pbbabreu/air-quality-brazil.git
cd air-quality-brazil
uv sync
```

Data files are not included in the repository due to size. Download instructions
are in `notebooks/01_data_collection.ipynb`.

## Author

Pedro Abreu — Mechanical Engineer transitioning into Data Science
[GitHub](https://github.com/pbbabreu)
