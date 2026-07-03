
# 🇮🇳 India AQI Predictor

**Predicting Air Quality Index across 26 Indian cities using 29,531 real CPCB government records — powered by XGBoost, with COVID-19 lockdown impact analysis, interactive geospatial mapping, and a live health alert dashboard.**

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-orange)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)
![Folium](https://img.shields.io/badge/Folium-Geospatial%20Viz-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## 📌 Overview

Air pollution is one of India's most pressing public health crises. This project builds an end-to-end machine learning pipeline that predicts daily Air Quality Index (AQI) across 26 Indian cities using **real, government-sourced pollution data** from the Central Pollution Control Board (CPCB) — not synthetic or simulated data.

Beyond prediction, the project answers three real-world questions:
- **Which cities and pollutants drive India's air quality crisis?**
- **Did the 2020 COVID-19 lockdown measurably improve air quality?**
- **Can we turn a raw ML model into a usable, human-readable health alert system?**

---

## 🎯 Key Results

| Metric | Train | Validation (2019) | Test (2020, unseen) |
|---|---|---|---|
| **MAE** (AQI points) | 16.55 | 20.98 | **17.37** |
| **RMSE** (AQI points) | 25.74 | 45.57 | **29.00** |
| **R² Score** | 0.9715 | 0.8873 | **0.8792** |
| **AQI Category Accuracy** | 80.5% | 74.7% | **74.3%** |

> Model was trained on 2015–2018 data, validated on 2019, and tested on a **fully unseen 2020 dataset** — the year of India's COVID-19 lockdown — to simulate genuine forecasting conditions rather than random train/test splits.

**Strongest per-city predictions:** Bengaluru (MAE 6.0), Kolkata (MAE 11.7), Delhi (MAE 15.8, R²=0.94) — despite Delhi having the most volatile, highest-magnitude AQI swings in the dataset.

---

## 🗂️ Dataset

| | |
|---|---|
| **Source** | [Kaggle — "Air Quality Data in India"](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india) by Rohan Rao |
| **Origin** | Central Pollution Control Board (CPCB), Government of India |
| **Records** | 29,531 daily observations (24,850 after cleaning) |
| **Cities** | 26 major Indian cities (Delhi, Mumbai, Bengaluru, Chennai, Kolkata, Hyderabad, and 20 more) |
| **Time Range** | 2015 – 2020 |
| **Features** | PM2.5, PM10, NO, NO2, NOx, NH3, CO, SO2, O3, Benzene, Toluene, AQI, AQI Bucket |

---

## ⚙️ Tech Stack

`Python` · `Pandas` · `NumPy` · `XGBoost` · `Scikit-learn` · `Matplotlib` · `Seaborn` · `Folium` (interactive geospatial maps) · `Jupyter Notebook`

---

## 🔬 Methodology

### 1. Data Cleaning & Missing Value Treatment
- Missing pollutant values imputed hierarchically: **city + month median → city median → global median**, preserving seasonal and regional pollution patterns instead of flattening them with a single global average.
- Rows with missing target (`AQI`) dropped to protect label integrity.

### 2. Feature Engineering
- **Cyclical time encoding** (sine/cosine transforms) for month, day-of-year, and day-of-week — so the model understands December and January are adjacent, not 11 months apart.
- **Pollutant interaction features**: PM2.5/PM10 ratio, total particulate load, NOx/NO2 ratio.
- **COVID lockdown flag** (April–June 2020) as an explicit binary feature.
- **City encoding** via LabelEncoder + year-normalized trend feature.

### 3. Model: XGBoost Regressor

    n_estimators=600, learning_rate=0.04, max_depth=8,
    subsample=0.8, colsample_bytree=0.8,
    min_child_weight=4, gamma=0.1,
    reg_alpha=0.1, reg_lambda=1.5,
    early_stopping_rounds=30

Trained with early stopping on a 2019 validation set to prevent overfitting; final evaluation on completely unseen 2020 data.

### 4. Evaluation
Beyond standard regression metrics (MAE, RMSE, R²), the model is scored on **AQI category accuracy** — whether it correctly classifies a day into India's official 6-tier health bucket (Good → Severe), which is what actually matters for public health alerts.

---

## 📊 Key Insights

**Pollutant drivers of AQI** (correlation strength):
CO (+0.68) and PM2.5 (+0.67) are the strongest predictors of AQI, followed by NO2 (+0.53) and PM10 (+0.51) — consistent with vehicular and industrial emissions being India's primary pollution sources.

**COVID-19 lockdown impact (2020):**
Cities showed a measurable AQI drop during the April–June 2020 lockdown, led by:
- Ahmedabad: **−53.1 AQI points**
- Chandigarh: **−47.3**
- Amaravati: **−39.2**
- Visakhapatnam: **−31.4**

This gives quantitative, data-backed evidence of the lockdown's air quality impact — not just a visual trend.

**Most polluted cities (2015–2020 average):** Ahmedabad and Delhi consistently rank highest, with Ahmedabad's real November 2019 AQI hitting **691 (Severe)**.

---

## 🗺️ Interactive Geospatial Map

Built with Folium (dark-themed CartoDB tiles) — plots all 26 cities on a live map of India, color-coded by average AQI severity with hover popups showing per-city pollutant breakdown and record counts.

📁 Output: `india_aqi_real_map.html` (open directly in browser)

---

## 🚨 Live Health Alert Dashboard

A rule-based layer on top of the ML model that converts raw AQI predictions into actionable guidance:

    predict_live_aqi("Delhi", year=2019, month=12, is_weekend=False)
    → 🟣 Very Poor | Respiratory illness risk on prolonged exposure
    → Advice: Stay indoors. Use air purifier if available.
    → Mask: N95 mandatory — even short outdoor exposure

Demoed across three real scenarios: Delhi winter smog, Kochi monsoon season, and Delhi during the COVID lockdown — showing the model correctly captures seasonal and event-driven AQI shifts.

---

## 📈 Visualizations

| Plot | Description |
|---|---|
| `city_aqi_real.png` | Average AQI ranking across all 26 cities |
| `trends_real.png` | Yearly + seasonal AQI trends (2015–2020) |
| `correlation_real.png` | Pollutant correlation heatmap |
| `covid_aqi_impact.png` | Delhi AQI before vs. during COVID lockdown |
| `model_eval_real.png` | Actual vs. predicted AQI, residual distribution |

---

## 🚀 How to Run

    # Clone the repo
    git clone https://github.com/haritha090/India-AQI-Predictor.git
    cd India-AQI-Predictor

    # Install dependencies
    pip install pandas numpy matplotlib seaborn scikit-learn xgboost folium opendatasets

    # Launch notebook
    jupyter notebook "AQI.ipynb"

> The notebook downloads the dataset directly from Kaggle via the `opendatasets` library — you'll need a free [Kaggle API token](https://www.kaggle.com/settings) on first run.

---

## 📁 Project Structure

    India-AQI-Predictor/
    ├── AQI.ipynb                   # Full pipeline: cleaning → EDA → modeling → dashboard
    ├── city_aqi_real.png
    ├── trends_real.png
    ├── correlation_real.png
    ├── covid_aqi_impact.png
    ├── model_eval_real.png
    └── README.md

---

## 🔮 Future Improvements
- Deploy the health alert dashboard as a live Streamlit web app
- Extend forecasting to multi-day-ahead predictions using time-series models (Prophet/LSTM)
- Add real-time AQI ingestion via a public CPCB/OpenAQ API instead of static historical data

---

## 👤 Author

**Haritha Kaki**
Data Analyst | Data Science | Python · SQL · Power BI
[LinkedIn](https://www.linkedin.com/in/kaki-haritha-93a3a8292/) · [GitHub](https://github.com/haritha090)

---

*Built on real government pollution data — because air quality models trained on synthetic data don't help anyone breathe easier.*
