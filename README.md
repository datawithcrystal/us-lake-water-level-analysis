# Correlation, Dependence and Forecasting of Water-Level Dynamics in Ten U.S. Lakes

## Project Overview

This repository contains the reproducibility materials for an MSc Data Science Extended Research Project investigating daily water-level dynamics across ten selected natural lakes in the United States from 2006 to 2025.

The study examines:

1. relationships between meteorological conditions and lake water levels;
2. contemporaneous, lagged and directional dependence among lakes; and
3. the forecasting performance of alternative time-series and machine-learning approaches.

The analyses include exploratory data analysis, Pearson and Spearman correlation, Cross-Correlation Function (CCF), Convergent Cross Mapping (CCM), Seasonal Naïve forecasting, Fourier-SARIMAX, climate-augmented forecasting, Vector Autoregression (VAR), VAR with exogenous meteorological predictors (VARX), and XGBoost.

---

## Study Lakes

The ten lakes included in the study are:

- Lake Superior
- Lake Huron
- Lake Michigan
- Lake Erie
- Lake Ontario
- Lake of the Woods
- Great Salt Lake
- Lake Tahoe
- Red Lake
- Lake Champlain

The study period is 1 January 2006 to 31 December 2025.

---

## Data Sources

Water-level data were obtained from:

- **NOAA Tides and Currents** (https://tidesandcurrents.noaa.gov/stations.html?type=Water+Levels) for Lake Superior, Lake Huron, Lake Michigan, Lake Erie and Lake Ontario.
- **United States Geological Survey (USGS) Water Data** (https://waterdata.usgs.gov/explore/#dataCollections=continuous) for the remaining five lakes.

Meteorological data were obtained from the **NOAA Global Historical Climatology Network Daily (GHCN-Daily)** (https://www.ncei.noaa.gov/cdo-web/search?datasetid=GHCND) dataset.

Detailed station identifiers, variables and retrieval information are provided in `data/README.md`.

The meteorological variables used in the study include:

- PRCP: daily precipitation (mm)
- TMAX: daily maximum temperature (°C)
- TMIN: daily minimum temperature (°C)
- TMEAN: mean daily temperature derived from processed TMAX and TMIN (°C)
- WESD: water equivalent of snow on the ground (mm)

Raw source data are not redistributed in this repository. Source and station information required to retrieve the original data are provided in the accompanying data documentation.

---

## Repository Structure

```text
.
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── 00_lake_location_map.ipynb
│   ├── 01_preprocessing_eda_dependence.ipynb
│   ├── 02_fourier_sarimax.ipynb
│   ├── 03_var_varx.ipynb
│   └── 04_xgboost.ipynb
│
├── data/
│   ├── README.md
│   ├── lake_station_metadata.csv
│   └── data_dictionary.csv
│
├── outputs/
│   └── [reference output files]
│
└── docs/
    └── technical_notes.md
