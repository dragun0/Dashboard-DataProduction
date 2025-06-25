# Dashboard Data Production Pipeline

This repository contains the full Python-based data preparation pipeline developed for the thesis project:

**Machine Learning vs. Physics-Based Weather Models: A Geospatial Assessment of AI’s Potential for More Accurate Weather Forecasting**  
*MSc Geoinformatics Engineering – Politecnico di Milano*  
Author: Leonie Dragun (10776334)

The code in this repository processes raw weather forecast and reanalysis data into verification datasets used in the forecast evaluation dashboard:  
🔗 [GC-dashboard (App Repo)](https://github.com/dragun0/GC-dashboard)  
🌐 [Live Dashboard (Deployed App)](https://gc-dashboard-red.vercel.app/)

---

## Purpose

This repository performs the fetching, preprocessing and processing of deterministic forecast data from three weather models:

- **ECMWF IFS-HRES** (Numerical Weather Prediction)
- **Google DeepMind GraphCast** (Machine Learning)
- **ECMWF AIFS** (Machine Learning)

...against the **ERA5 reanalysis** ground truth, to compute forecast error metrics (RMSE, MAE, MBE, R), and convert the results into:

- Multi-scale Zarr pyramids for interactive maps  
- Region-aggregated JSON files for time series (graphs) visualisations

---

## Main Components

### Jupyter Notebooks

Modular Jupyter Notebooks for each stage of the data production pipeline:

| Notebook | Function |
|----------|----------|
| `GraphCast_WeatherForecasting/GraphCast_Forecasting.ipynb` | Produce GraphCast forecasts using [remote-graphcast]([https://github.com/dragun0/GC-dashboard](https://github.com/dragun0/remote-graphcast-CDSapi-0.7.x)) and upload to S3 |
| `DataFetch_Preprocessing_Processing/GraphCast_DataFetch.ipynb` | Download GraphCast prediction subsets from S3 |
| `DataFetch_Preprocessing_Processing/MARS_CDS_DataFetch - Surface Variables.ipynb` | Download IFS-HRES, AIFS surface forecasts from MARS and ERA5 from CDS |
| `DataFetch_Preprocessing_Processing/MARS_CDS_DataFetch - Pressure Variables.ipynb` | Download pressure-level variables from MARS and CDS |
| `DataFetch_Preprocessing_Processing/DataPreprocessing_TimeFix_LongitudeFix.ipynb` | Apply preprocessing (temporal fixes, longitude alignment) |
| `DataFetch_Preprocessing_Processing/Compute_Monthly_AE.ipynb` | Compute Absolute Error for each lead time of each month (spatial, per pixel) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/Maps/Spatial_MAE_leadtimes.ipynb` | Compute MAE at each lead time over all months (spatial, per pixel) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/Maps/Spatial_MBE_leadtimes.ipynb` | Compute MBE at each lead time over all months (spatial, per pixel) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/Maps/Spatial_RMSE_Leadtimes.ipynb` | Compute RMSE at each lead time over all months (spatial, per pixel) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/MAE_LeadTimes.ipynb` | Compute MAE per region for each lead time over all months and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/MAE_Monthly.ipynb` | Compute MAE per region for each month over all lead times and all grid points (non-spatial)|
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/MBE_LeadTimes.ipynb` | Compute MBE per region for each lead time over all months and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/MBE_Monthly.ipynb` | Compute MBE per region for each month over all lead times and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/R_LeadTimes.ipynb` | Compute correlation (R) per region for each lead time over all months and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/R_Monthly.ipynb` | Compute correlation (R) per region for each month over all lead times and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/RMSE_LeadTimes.ipynb` | Compute RMSE per region for each lead time over all months and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/RMSE_Monthly.ipynb` | Compute RMSE per region for each month over all lead times and all grid points (non-spatial) |
| `DataFetch_Preprocessing_Processing/Zarr_Pyramid_Creation.ipynb` | Convert spatial forecast errors to multiscale Zarr pyramids (for visualisation with @carbonplan/maps) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/Maps/Minimaps_Zarr_creation.ipynb` | Convert spatial forecast errors to single-scale Zarr (for visualisation with @carbonplan/minimaps) |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/X/X/x_to_JSON_Monthly.ipynb` | Export non-spatial monthly metrics to JSON |
| `DataFetch_Preprocessing_Processing/PlotsPage_Data/X/X/x_to_JSON_LeadTime.ipynb` | Export non-spatial lead time metrics to JSON |


## Outputs

Processed outputs are saved as:

- `.zarr` files → Cloud-optimised spatial forecast accuracy tiles
- `.json` files → Monthly/lead-time metric summaries per region

---

## Requirements

Create a virtual environment and install the dependencies:

```bash
conda create -n forecast_env python=3.10
conda activate forecast_env
pip install -r requirements.txt
```

---

## Usage

1. Generate GraphCast predictions 
2. Download IFS-HRES and AIFS forecasts and ERA5
3. Preprocess datasets (unit, longitude conversions etc.)
4. Compute verification metrics
5. Export Zarr + JSON files

---

## Related Projects

- 📊 [GC-dashboard](https://github.com/dragun0/GC-dashboard)
- 🌐 [Live Dashboard Deployment](https://gc-dashboard-red.vercel.app/)

---
