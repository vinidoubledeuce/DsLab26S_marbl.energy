<p align="center">
  <img src="marbl.png" alt="MARBL Energy" width="200"/>
</p>

# Electricity Price Trends and Market Volatility in Europe

A data science project analyzing electricity price dynamics and market volatility across three European bidding zones with different generation profiles, featuring an interactive Streamlit dashboard with ML-powered day-ahead price forecasting.

---

## Overview

This project investigates electricity price patterns in European wholesale markets, developed for the **Data Science Lab 2025/26** at WU Vienna in collaboration with **Marbl FlexCo**.

### Bidding Zones

| Zone | Region | Primary Generation | Characteristics |
|------|--------|-------------------|-----------------|
| **DK1** | Denmark West | Wind (55%) | High volatility, strong seasonal patterns |
| **ES** | Spain | Solar | Midday price dips, summer peaks |
| **NO2** | South Norway | Hydro | Stable baseload, interconnector hub |

---

## Features

- **Historical Data Exploration** — Interactive visualization of 3 years of price and weather data
- **Pattern Detection** — Hierarchical clustering identifies recurring daily price profiles
- **Day-Ahead Forecasting** — XGBoost Mix-of-Experts model predicts next-day hourly prices
- **Live Dashboard** — Streamlit web app with real-time forecasts and market insights

---

## Project Structure

```
DsLab25W_marbl.energy/
├── dashboard/                  # Streamlit web application
│   ├── app.py                 # Landing page
│   ├── pages/                 # Dashboard pages
│   │   ├── 1_Market_Overview.py
│   │   ├── 2_Cluster_Analysis.py
│   │   └── 3_Live_Forecast.py
│   └── utils/                 # Helper modules
│
├── notebooks/                  # Analysis pipeline
│   ├── 01_ingestion/          # Data collection (ENTSO-E, ERA5)
│   ├── 02_preprocessing/      # Cleaning & merging
│   ├── 03_analysis/           # Pattern detection
│   └── 04_predictions/        # Model training
│
├── models/                     # Trained XGBoost models
│   └── approach_clusters/     # Mix-of-Experts models
│
├── data/
│   ├── processed/             # Final mastersets (tracked in git)
│   └── raw/                   # Raw API data (not tracked)
│
├── config/
│   └── secrets.env            # API credentials (not tracked)
│
└── requirements.txt           # Python dependencies
```

---

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/builtbymaxim/DsLab25W_marbl.energy.git
cd DsLab25W_marbl.energy
```

### 2. Create Virtual Environment

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Dashboard

```bash
streamlit run dashboard/app.py
```

The dashboard will open at `http://localhost:8501`

---

## Configuration

To run the full data pipeline (notebooks), you need API credentials in `config/secrets.env`:

```env
# ENTSO-E Transparency Platform (prices)
ENTSOE_TOKEN=your_token_here

# Copernicus Climate Data Store (weather history)
CDS_URL=https://cds.climate.copernicus.eu/api
CDS_KEY=your_key_here

# WeatherAPI.com (live forecasts)
WEATHER_API_KEY=your_key_here
```

**Note:** The dashboard works without API keys using pre-processed data in `data/processed/`.

---

## Data Pipeline

The analysis pipeline consists of four stages:

| Stage | Notebooks | Purpose |
|-------|-----------|---------|
| **01_ingestion** | `01_prices_pd-pkg.ipynb`, `02_weather_era5.ipynb` | Collect prices from ENTSO-E and weather from ERA5 |
| **02_preprocessing** | `02_weather_and_merge.ipynb` | Clean data, handle DST, create mastersets |
| **03_analysis** | `{zone}_pattern_detection.ipynb` | Hierarchical clustering for price patterns |
| **04_predictions** | `Predictions.ipynb` | Train XGBoost Mix-of-Experts models |

---

## Dashboard Pages

### Market Overview
Explore historical price and weather data with interactive time series, daily profiles, and correlation analysis.

### Cluster Analysis
Visualize identified price patterns (clusters) with centroid profiles, seasonal distributions, and calendar views.

### Live Forecast
Generate day-ahead price predictions using trained ML models with weather forecast inputs.

---

## Model Architecture

The forecasting system uses a **Mix-of-Experts** approach:

1. **Cluster Classifier** — Predicts which price pattern (cluster) tomorrow will follow
2. **Price Regressors** — Specialized models predict hourly prices for each cluster type

This architecture captures distinct market regimes (e.g., high-wind days vs. calm days) better than a single model.

---

## Data Attribution

- **Price Data:** [ENTSO-E Transparency Platform](https://transparency.entsoe.eu/) — Licensed under CC-BY 4.0
- **Weather History:** Contains modified Copernicus Climate Change Service information (ERA5)
- **Weather Forecasts:** [WeatherAPI.com](https://www.weatherapi.com/)

---

## Team

| Name | Responsibility |
|------|----------------|
| Maximilian Dieringer | Cluster Analysis |
| Harald Körbel | Prediction Modelling |
| Maxim Gomez Valverde | Ingestion, Preprocessing & Dashboard |
| Daniel Klaric | Ingestion & Prediction Modelling |

---

## License

This project was developed for academic purposes as part of the WU Vienna Data Science Lab.
