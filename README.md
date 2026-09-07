# Correlation, Dependence and Forecasting of Water-Level Dynamics in Ten U.S. Lakes

## Project Overview

This repository contains the reproducibility materials for an MSc Data Science dissertation investigating daily water-level dynamics across ten selected natural lakes in the United States over the period 2006–2025.

The study examines:

- temporal and seasonal characteristics of lake water levels;
- relationships between meteorological variables and water levels;
- contemporaneous and lagged dependence between lakes;
- nonlinear directional dependence using Convergent Cross Mapping (CCM); and
- out-of-sample water-level forecasting using statistical, multivariate and machine-learning models.

The analytical framework includes exploratory data analysis, Pearson and Spearman correlation, cross-correlation functions (CCF), CCM, seasonal naïve forecasting, Fourier-SARIMAX, VAR/VARX and XGBoost.

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

The sample includes both hydrologically connected lakes within the Laurentian Great Lakes system and geographically separated lakes with different climatic and hydrological settings.

---

## Data Sources

Water-level observations were obtained from:

- **NOAA Tides & Currents** for Lakes Superior, Huron, Michigan, Erie and Ontario; and
- **USGS Water Data** for Lake of the Woods, Great Salt Lake, Lake Tahoe, Red Lake and Lake Champlain.

Meteorological observations were obtained from **NOAA/NCEI Global Historical Climatology Network Daily (GHCN-Daily)** stations selected to provide representative climate information for each lake.

The principal meteorological variables used in the analysis are:

- precipitation (`PRCP`);
- maximum temperature (`TMAX`);
- minimum temperature (`TMIN`);
- mean temperature (`TMEAN`); and
- water-equivalent snow depth (`WESD`), where available.

Detailed information on monitoring stations, station identifiers, expected source filenames, variables and preprocessing is provided in [`data/README.md`](data/README.md).

---

## Repository Structure

```text
us-lake-water-level-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── README.md
│   ├── ERP_00_Lake_Location_Map.ipynb
│   ├── ERP_01_preprocessing_eda_dependence.ipynb
│   ├── ERP_02_fourier_sarimax.ipynb
│   ├── ERP_03_var_varx.ipynb
│   └── ERP_04_xgboost.ipynb
├── data/
│   ├── README.md
│   ├── metadata/
│   │   ├── README.md
│   │   └── Lake Metadata.xlsx
│   └── processed/
│       ├── README.md
│       └── water_climate_10_lakes_primary.csv
└── outputs/
    ├── figures/
    ├── ccm_results/
    └── forecasting_results/
```

Raw source data are not redistributed in this repository. Information required to retrieve and organise the original source files is provided in `data/README.md`.

---

## Notebook Contents

### `ERP_00_Lake_Location_Map.ipynb`

Creates the geographical visualisation of the ten study lakes and their associated water-level and meteorological monitoring stations.

### `ERP_01_preprocessing_eda_dependence.ipynb`

Performs:

- water-level and meteorological data preprocessing;
- missing-data treatment;
- construction of the combined processed dataset;
- exploratory data analysis;
- seasonal and distributional analysis;
- climate–water-level Pearson and Spearman correlations;
- climate–water-level cross-correlation analysis;
- inter-lake correlation and cross-correlation analysis; and
- monthly Convergent Cross Mapping analysis.

This notebook produces the processed dataset used by the downstream forecasting notebooks and exports selected CCM results.

### `ERP_02_fourier_sarimax.ipynb`

Implements the lake-specific statistical forecasting framework, including:

- time-series diagnostics;
- Augmented Dickey–Fuller tests;
- seasonal decomposition;
- ACF and PACF assessment;
- seasonal naïve benchmarks;
- Fourier-SARIMAX model selection;
- climate-augmented Fourier-SARIMAX models;
- validation-based model selection;
- final test-period evaluation; and
- residual diagnostics.

### `ERP_03_var_varx.ipynb`

Implements multivariate forecasting for the five Laurentian Great Lakes using:

- VAR models based on first-differenced water levels;
- validation-based lag selection;
- VARX models incorporating meteorological predictors; and
- independent test-period comparison of VAR and VARX forecasts.

### `ERP_04_xgboost.ipynb`

Implements recursive XGBoost forecasting using:

- lagged lake-level predictors;
- annual sine and cosine terms;
- contemporaneous precipitation and mean temperature;
- validation-based hyperparameter selection; and
- final out-of-sample evaluation.

The notebook also produces the final cross-model comparison across the ten study lakes.

---

## Forecasting Design

To preserve temporal ordering and reduce information leakage, forecasting was evaluated using fixed chronological periods:

- **Training:** 2006–2018
- **Validation:** 2019–2021
- **Final refit:** 2006–2021
- **Test:** 2022–2025

Model specifications and hyperparameters were selected using the training and validation periods only. Selected models were then refitted using data through the end of 2021 before final evaluation on the independent 2022–2025 test period.

Climate-augmented SARIMAX, VARX and XGBoost forecasts use observed precipitation and mean temperature over the forecast horizon and should therefore be interpreted as **conditional forecasts** rather than forecasts based on independently predicted meteorological variables.

---

## Key Methodological Settings

Important preprocessing and modelling settings include:

- daily study period: 1 January 2006 to 31 December 2025;
- water-level gaps of 14 days or fewer were linearly interpolated;
- longer water-level gaps were retained as missing;
- internal `TMAX` and `TMIN` gaps of 7 days or fewer were linearly interpolated;
- precipitation was not interpolated;
- `TMEAN` was calculated from `TMAX` and `TMIN`;
- `WESD` was retained separately and was not interpolated;
- the seasonal naïve benchmark used a 365-day lag;
- Fourier terms used an annual period of 365.25 days;
- climate-lag selection was performed using training data only;
- VAR/VARX analysis was restricted to the five Laurentian Great Lakes;
- XGBoost used lake-level lags of 1, 7, 14, 30 and 365 days;
- CCM was performed using monthly lake-level series derived from the daily observations; and
- randomised CCM procedures used a fixed seed for reproducibility.

Further methodological details are documented directly within the notebooks and in the dissertation.

---

## Software Environment

The analysis was developed using:

```text
Python 3.9.13
```

The principal Python packages and their versions are listed in:

```text
requirements.txt
```

A compatible environment can be created using:

```bash
pip install -r requirements.txt
```

---

## How to Reproduce the Analysis

### Option 1: Reproduce from the original source data

To reproduce the complete workflow from the original NOAA, USGS and GHCN-Daily files:

1. Download the source data described in [`data/README.md`](data/README.md).
2. Place the files in the directory structure specified there.
3. Run the notebooks in the following order:

```text
1. notebooks/ERP_00_Lake_Location_Map.ipynb
2. notebooks/ERP_01_preprocessing_eda_dependence.ipynb
3. notebooks/ERP_02_fourier_sarimax.ipynb
4. notebooks/ERP_03_var_varx.ipynb
5. notebooks/ERP_04_xgboost.ipynb
```

Notebook 01 performs the primary preprocessing and creates the processed dataset used by the forecasting notebooks.

### Option 2: Reproduce the downstream analyses from the processed dataset

The processed dataset used in the downstream analyses is provided at:

```text
data/processed/water_climate_10_lakes_primary.csv
```

This allows the forecasting analyses in Notebooks 02–04 to be reproduced without separately downloading and preprocessing the original source data.

The processed dataset contains the principal fields:

```text
Date
Lake
Lake_Level_ft
PRCP
TMAX
TMIN
TMEAN
WESD
```

For complete preprocessing reproducibility, refer to Notebook 01 and `data/README.md`.

---

## Reference Outputs

Selected analytical outputs are retained to support verification of the results reported in the dissertation.

### Figures

```text
outputs/figures/
```

Contains selected figures used in the dissertation and supporting analytical visualisations.

### CCM Results

```text
outputs/ccm_results/
```

Contains selected CCM outputs, including embedding-dimension results, pair-level summaries, directional summaries, convergence information and representative results.

### Forecasting Results

```text
outputs/forecasting_results/
```

Contains selected outputs supporting model selection, validation and final test-period comparisons, including Fourier-SARIMAX, climate-augmented models, VAR/VARX and XGBoost results.

These outputs are provided as reference materials rather than as substitutes for executing the notebooks.

---

## Reproducibility Notes

The repository is intended to provide a clear and proportionate reproducibility package for the analyses reported in the dissertation.

Original NOAA, USGS and GHCN-Daily source files are not redistributed. Instead, the repository provides:

- source organisations and data products;
- monitoring-station identifiers;
- expected source filenames;
- lake metadata;
- preprocessing rules;
- the processed dataset used by the downstream analyses;
- executable analysis notebooks;
- the software environment specification; and
- selected analytical outputs used to verify reported results.

Detailed source-data documentation is available in [`data/README.md`](data/README.md).
