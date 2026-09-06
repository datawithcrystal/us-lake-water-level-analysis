# Data Documentation

## Overview

This directory contains the processed dataset used in the forecasting analyses and documentation for retrieving the original water-level and meteorological source data.

Raw source data are not redistributed in this repository. They can be retrieved from NOAA Tides and Currents, USGS Water Data and NOAA/NCEI GHCN-Daily using the station information provided below.

For a complete reproduction of Notebook 01, downloaded source files should be placed in:

```text
data/
└── raw data/
    ├── water levels/
    └── climate variables/
```
# Study Period

The common study period is:

### 1 January 2006 to 31 December 2025

# Water-Level Data

Water-level observations for the five Laurentian Great Lakes were obtained from NOAA Tides and Currents.

Water-level observations for Lake of the Woods, Great Salt Lake, Lake Tahoe, Red Lake and Lake Champlain were obtained from USGS Water Data.

| Lake              | Source | Station ID | Expected filename                                        |
| ----------------- | ------ | ---------- | -------------------------------------------------------- |
| Lake Superior     | NOAA   | 9099064    | Lake Superior 2006~2015.csv; Lake Superior 2016~2025.csv |
| Lake Huron        | NOAA   | 9075080    | Lake Huron 2006~2015.csv; Lake Huron 2016~2025.csv       |
| Lake Michigan     | NOAA   | 9087057    | Lake Michigan 2006~2015.csv; Lake Michigan 2016~2025.csv |
| Lake Erie         | NOAA   | 9063020    | Lake Erie 2006~2015.csv; Lake Erie 2016~2025.csv         |
| Lake Ontario      | NOAA   | 9052058    | Lake Ontario 2006~2015.csv; Lake Ontario 2016~2025.csv   |
| Lake of the Woods | USGS   | 05140520   | Lake Woods.csv                                           |
| Great Salt Lake   | USGS   | 10010000   | Great Salt Lake.csv                                      |
| Lake Tahoe        | USGS   | 10337000   | Lake Tahoe.csv                                           |
| Red Lake          | USGS   | 05074000   | Red Lake.csv                                             |
| Lake Champlain    | USGS   | 04294500   | Lake Champlain.csv                                       |
Within the analytical code, Lake Woods is used as the internal label for Lake of the Woods.

# Meteorological Data

Meteorological observations were obtained from NOAA/NCEI GHCN-Daily.

The source files should be placed in:
data/raw data/climate variables/

The expected filenames are:
| Lake | GHCN-Daily Station ID | Station Name | Expected filename |
|---|---|---|---|
| Lake Superior | GHCND:USW00014913 | DULUTH INTERNATIONAL AIRPORT, MN US | Lake_Superior.csv |
| Lake Huron | GHCND:USW00014841 | PELLSTON REGIONAL AIRPORT, MI US | Lake_Huron.csv |
| Lake Michigan | GHCND:USW00014839 | MILWAUKEE MITCHELL AIRPORT, WI US | Lake_Michigan.csv |
| Lake Erie | GHCND:USW00014733 | BUFFALO NIAGARA INTERNATIONAL AIRPORT, NY US | Lake_Erie.csv |
| Lake Ontario | GHCND:USW00014768 | FREDERICK DOUGLASS GREATER ROCHESTER INTERNATIONAL AIRPORT, NY US | Lake_Ontario.csv |
| Lake of the Woods | GHCND:USW00094961 | BAUDETTE INTERNATIONAL AIRPORT, MN US | Lake_Woods.csv |
| Great Salt Lake | GHCND:USW00024127 | SALT LAKE CITY INTERNATIONAL AIRPORT, UT US | Great_Salt_Lake.csv |
| Lake Tahoe | GHCND:USS0020K27S | TAHOE CITY CROSS, CA US | Lake_Tahoe.csv |
| Red Lake | GHCND:USC00216787 | RED LAKE FALLS, MN US | Red_Lake.csv |
| Lake Champlain | GHCND:USW00014742 | BURLINGTON INTERNATIONAL AIRPORT, VT US | Lake_Champlain.csv |

The corresponding GHCN-Daily station identifiers should be documented here using the final stations used in Notebook 01.

# Meteorological Variables
PRCP: daily precipitation (mm)
TMAX: daily maximum temperature (°C)
TMIN: daily minimum temperature (°C)
TMEAN: daily mean temperature calculated from processed TMAX and TMIN
WESD: water equivalent of snow on the ground (mm)

# Processed Dataset

Notebook 01 generates:

data/processed/water_climate_10_lakes_primary.csv

This is the primary input dataset used by Notebooks 02–04.

The processed dataset contains the following variables:

| Variable      | Description                            |
| ------------- | -------------------------------------- |
| Date          | Observation date                       |
| Lake          | Lake identifier                        |
| Lake_Level_ft | Daily lake water level in feet         |
| PRCP          | Daily precipitation                    |
| TMAX          | Daily maximum temperature              |
| TMIN          | Daily minimum temperature              |
| TMEAN         | Derived daily mean temperature         |
| WESD          | Water equivalent of snow on the ground |

# Missing-Data Processing

Water-level gaps of 14 days or fewer were linearly interpolated, while longer gaps were retained as missing.

For meteorological data, internal TMAX and TMIN gaps of up to 7 days were linearly interpolated. Longer gaps were retained. PRCP was not interpolated. TMEAN was recalculated from the processed TMAX and TMIN series.

Further preprocessing details are implemented in 01_preprocessing_eda_dependence.ipynb.













