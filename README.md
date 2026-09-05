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
lake water levels dissertation
│
├── README.md
│
├── notebooks/
│   ├── 01_preprocessing_eda_dependence.ipynb
│   ├── 02_fourier_sarimax.ipynb
│   ├── 03_var_varx.ipynb
│   └── 04_xgboost.ipynb
│
├── Data/
│   ├── Lake Woods.csv
│   ├── Great Salt Lake.csv
│   ├── Lake Champlain.csv
│   ├── Red Lake.csv
│   ├── Lake Tahoe.csv
│   │
│   ├── Lake Superior 2006~2015.csv
│   ├── Lake Superior 2016~2025.csv
│   ├── Lake Huron 2006~2015.csv
│   ├── Lake Huron 2016~2025.csv
│   ├── Lake Michigan 2006~2015.csv
│   ├── Lake Michigan 2016~2025.csv
│   ├── Lake Erie 2006~2015.csv
│   ├── Lake Erie 2016~2025.csv
│   ├── Lake Ontario 2006~2015.csv
│   ├── Lake Ontario 2016~2025.csv
│   │
│   ├── Climate Variables/
│   │   ├── Lake_Superior.csv
│   │   ├── Lake_Huron.csv
│   │   ├── Lake_Michigan.csv
│   │   ├── Lake_Erie.csv
│   │   ├── Lake_Ontario.csv
│   │   ├── Lake_Woods.csv
│   │   ├── Great_Salt_Lake.csv
│   │   ├── Lake_Tahoe.csv
│   │   ├── Red_Lake.csv
│   │   └── Lake_Champlain.csv
│   │
│   └── processed/
│       └── water_climate_10_lakes_primary.csv
│
└── outputs/
```
---

## Notebook Contents

**00_lake_location_map.ipynb**  
Produces the geographical study-location figure.

**01_preprocessing_eda_dependence.ipynb**  
Contains water-level and meteorological preprocessing, exploratory data analysis, climate–water-level correlation and CCF analysis, inter-lake correlation and CCF analysis, and CCM.

**02_fourier_sarimax.ipynb**  
Contains time-series diagnostics, the Seasonal Naïve benchmark, annual SARIMA feasibility assessment, Fourier-SARIMAX model selection, climate predictor assessment and residual diagnostics.

**03_var_varx.ipynb**  
Contains VAR and VARX modelling for the five Laurentian Great Lakes, including lag-order selection and final forecast evaluation.

**04_xgboost.ipynb**  
Contains XGBoost feature construction, hyperparameter selection, recursive forecasting and cross-model forecast comparison.

---

## Forecasting Design

All forecasting analyses followed a chronological training-validation-testing framework:

- **Parameter training:** 2006–2018
- **Validation and model selection:** 2019–2021
- **Final refit:** 2006–2021
- **Independent testing:** 2022–2025

Forecast accuracy was evaluated using RMSE, MAE, R² and NRMSE.

Observed `PRCP` and `TMEAN` values from the forecast horizon were used in the climate-augmented SARIMAX, VARX and XGBoost analyses. These models should therefore be interpreted as **conditional forecasts under known meteorological conditions**, rather than fully operational forecasts based on independently forecast meteorological inputs.

---

## Key Methodological Settings

### Climate and Inter-Lake CCF

- Primary lag window: **0–30 days**
- Adaptive extension to **60 and 90 days** where the strongest correlation remained at the search-window boundary
- Seasonal-anomaly sensitivity analysis for unresolved climate–water-level relationships
- First-difference sensitivity analysis for extended inter-lake relationships

### Convergent Cross Mapping (CCM)

- Daily water levels aggregated to monthly means
- **240 monthly observations** per lake
- Candidate embedding dimensions: **E = 1–7**
- Library sizes: **20–220 months** in 20-month increments
- **20 random library samples** per library size
- `Tp = 0`
- `τ = −1`

### Fourier-SARIMAX

- Annual Fourier period: **365.25 days**
- Candidate Fourier orders: **K = 1–6**
- Candidate ARIMA orders: **p, q = 0–2**
- Validation RMSE used as the primary model-selection criterion
- Specifications within **1% of the minimum validation RMSE** were treated as near-best, with simpler specifications preferred

### VAR and VARX

- Applied to the five Laurentian Great Lakes
- Candidate lag orders: **1, 2, 3, 5, 7, 10, 14, 21, 28 and 30 days**
- Lag selection based on mean validation NRMSE across the five lakes
- Lag structures within **1% of the minimum mean validation NRMSE** were treated as near-best, with the shorter lag preferred
- VARX added contemporaneous `PRCP` and `TMEAN` to the selected VAR structure

### XGBoost

- Water-level lags: **1, 7, 14, 30 and 365 days**
- Annual sine and cosine terms based on a **365.25-day period**
- Contemporaneous `PRCP` and `TMEAN`
- Five predefined hyperparameter configurations evaluated separately for each lake
- Hyperparameters selected using the lowest validation RMSE
- Recursive multi-step forecasting used during validation and testing

Further implementation details and parameter settings are documented within the notebooks.

---

## Software Environment

The analysis was conducted in Python using Jupyter Notebook.

Main packages include:

- `pandas`
- `numpy`
- `scipy`
- `matplotlib`
- `statsmodels`
- `scikit-learn`
- `xgboost`
- `pyEDM`

Exact tested package versions are listed in [`requirements.txt`](requirements.txt).

---

## Reference Outputs

Selected final analytical outputs are provided in the [`outputs/`](outputs/) directory to support verification of reproduced results.

Intermediate debugging files, superseded model versions and analyses not reported in the dissertation are intentionally excluded.

---

## Reproducibility Notes

This repository contains the final analytical workflow corresponding to the results reported in the dissertation. It is intended to provide sufficient information to reproduce the reported analyses without including superseded notebooks, exploratory trials or other materials that do not contribute to the final reported results.
