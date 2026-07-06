# 🇮🇳 India AQI Predictor

# 🌫️ India AQI Predictor

Predicting Air Quality Index across Indian cities using 29,531 real CPCB government records — with interactive maps, COVID lockdown analysis, and a live health alert dashboard.

![Python](https://img.shields.io/badge/Python-3.10-blue)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## 📌 Overview

This project builds an end-to-end AQI prediction pipeline using real historical pollution data published by India's Central Pollution Control Board (CPCB). It covers 26 cities from 2015-2020, including the COVID-19 lockdown period, and includes a full ML pipeline, geospatial visualization, and city-wise health alert analysis.

## 🎯 Key Results

Metric              | Train  | Val (2019) | Test (2020)
-------------------- | ------ | ---------- | -----------
MAE (AQI pts)         | 16.55  | 20.98      | 17.37
RMSE                  | 25.74  | 45.57      | 29.00
R² Score              | 0.9715 | 0.8873     | 0.8792
Category Accuracy     | 80.5%  | 74.7%      | 74.3%

- 74.3% AQI category accuracy and R² = 0.88 on completely unseen 2020 data, using a strict time-based train/val/test split.
- Detected up to a 53-point AQI improvement across cities during the April-June 2020 COVID lockdown.

## 🗂️ Dataset

- Source: CPCB (Central Pollution Control Board), India - government air quality monitoring data
- Size: 29,531 cleaned records
- Coverage: 26 cities, 2015-2020
- Pollutants used: PM2.5, PM10, NO2, SO2, CO, O3, and available NH3/NOx

## ⚙️ Approach

1. Data Cleaning - handled missing values, outliers, and inconsistent city/pollutant labels
2. Feature Engineering
   - Cyclical encoding of month, day-of-year, day-of-week
   - Pollutant interaction features (PM ratio, PM total)
   - COVID lockdown flag (2020, April-June)
   - Year-normalized trend + encoded city
3. Modeling - XGBoost Regressor, trained on 2015-18, validated on 2019, tested on unseen 2020 data
4. Evaluation - MAE, RMSE, R², and AQI category classification accuracy (Good to Severe)

## 📊 Visualizations & Deliverables

- Interactive Folium map of city-wise AQI (india_aqi_real_map.html)
- Actual vs predicted AQI scatter plot, colored by health category
- Residual error distribution and feature importance chart
- Per-city model performance comparison
- COVID-19 lockdown AQI impact analysis
- Health alert dashboard logic based on predicted AQI category

## 🛠️ Tech Stack

Python, Pandas, NumPy, XGBoost, Scikit-learn, Matplotlib, Folium

## 📁 Repository Structure

India-AQI-Predictor/
- AQI_Predictor.ipynb - Full notebook: cleaning, features, model, evaluation
- india_aqi_xgb_real.pkl - Trained XGBoost model
- india_aqi_cpcb_cleaned.csv - Cleaned CPCB dataset
- india_aqi_real_map.html - Interactive Folium geospatial dashboard
- model_eval_real.png - Model evaluation plots (scatter, residuals, feature importance)
- covid_aqi_impact.png - COVID lockdown AQI impact analysis
- city_aqi_real.png - City-wise pollution ranking
- trends_real.png - Yearly and seasonal AQI trends
- correlation_real.png - Pollutant correlation heatmap
- README.md

## ▶️ How to Run

git clone https://github.com/haritha090/India-AQI-Predictor.git
cd India-AQI-Predictor
pip install -r requirements.txt
jupyter notebook AQI_Predictor.ipynb

## 🚀 Future Improvements

- Real-time AQI prediction via live API integration
- LSTM-based time-series forecasting
- Deploy as a Streamlit web app for public use

## 👤 Author

Haritha Kaki
Email: harithak08047@gmail.com
GitHub: https://github.com/haritha090

   
