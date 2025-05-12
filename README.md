# Electric Load Forecasting Using Data Mining Techniques

## Authors

* Muhammad Qasim (22I-1994)
* Ayaan Khan (22I-2066)
* Abu Bakr Nadeem (22I-2003)

---

## 1. Project Overview

Electric load forecasting is crucial for effective energy management, ensuring grid reliability, and facilitating demand response. This project presents a full-stack data mining and machine learning solution for forecasting hourly electricity demand, using weather and demand data from ten major U.S. cities.

### Objectives

1. *Cluster Analysis*: Discover latent usage patterns by clustering observations based on weather and demand.
2. *Predictive Modeling*: Develop and compare forecasting models including statistical, ML, and deep learning approaches.
3. *Web Interface*: Create an interactive UI for parameter selection, clustering visualization, and forecast display.

---

## 2. Dataset Description

### Components

* *Weather Data*: Hourly JSON files per city (10 cities) with temperature, humidity, wind speed, pressure, and dew point.
* *Demand Data*: Hourly electricity demand in MWh from 3 CSV files across 6 regions.

### Merged Schema (after preprocessing)

| Column      | Type        | Description                     |
| ----------- | ----------- | ------------------------------- |
| timestamp   | datetime    | Hourly timestamp (UTC)          |
| city        | categorical | City name                       |
| temperature | float       | Ambient temperature (°F)        |
| humidity    | float       | Relative humidity (%)           |
| windSpeed   | float       | Wind speed (mph)                |
| pressure    | float       | Atmospheric pressure (hPa)      |
| dewPoint    | float       | Dew point temperature (°F)      |
| demand\_mwh | float       | Hourly electricity demand (MWh) |

---

## 3. Data Preprocessing

* *Loading & Parsing*: Weather JSONs and demand CSVs parsed and merged on timestamp and city.
* *Missing Values*: \~3% missing, removed records with missing demand, imputed weather with median.
* *Feature Engineering*: Extracted time features, applied RobustScaler.
* *Aggregation*: Weekly summaries per city (mean weather, demand stats).
* *Anomaly Detection*: IsolationForest used; anomalies imputed.

> Preprocessed data saved to preprocessed_and_cleaned_data.csv.

---

## 4. Cluster Analysis

* *Features Used*: Temperature, humidity, wind speed, pressure, dew point, demand.
* *Dimensionality Reduction*: PCA to 2D.
* *Clustering*: K-Means with k=3 (based on Elbow method).

### Cluster Interpretations

| Cluster | Label                        | Description                    |
| ------- | ---------------------------- | ------------------------------ |
| 0       | Cool Midday, Moderate Demand | Mild weather, steady usage     |
| 1       | Hot Afternoon, High Demand   | Peak hours, high cooling loads |
| 2       | Humid Morning, Low Demand    | Early morning, lower demand    |

* *Silhouette Score*: 0.284
* *Export*: Cluster assignments saved and visualized via PCA scatter plot.

---

## 5. Predictive Modeling

### Models Implemented

1. *Naïve Baseline*: Yesterday's demand.
2. *Linear Regression*: Simple regression with tuning.
3. *SARIMA*: Seasonal ARIMA (Phoenix example).
4. *XGBoost*: Tree-based ensemble method.
5. *LSTM*: Deep learning with temporal features.

### Training & Validation

* *Chronological Train/Test Split*: 80/20
* *Scaling*: RobustScaler; encoding for categorical variables
* *Evaluation Metrics*: MAE, RMSE, MAPE, R²

### Results Summary

| Model             | R²     | MAE    | RMSE   | Notes                       |
| ----------------- | ------ | ------ | ------ | --------------------------- |
| Linear Regression | 0.8763 | 0.1295 | 0.1816 | Baseline                    |
| XGBoost           | 0.9360 | 0.0729 | 0.1307 | Strong performance          |
| LSTM              | 0.9648 | 0.0663 | 0.1137 | Best overall                |
| SARIMA (Phoenix)  | 0.8528 | 155.34 | 338.94 | Limited to daily resolution |

### Ensemble

* Combined XGBoost + LSTM improved RMSE by \~2%.

---

## 6. Front-End Interface

### Technologies

* *Backend*: Flask
* *Frontend*: HTML/CSS/JavaScript

### Features

* *City & Model Selection*: Dropdowns, checkboxes, sliders
* *Visualization*: PCA scatter plots, forecast graphs
* *Download Options*: Forecast CSVs
* *User Help*: Inline tooltips, metric descriptions

---

## 7. Conclusion & Recommendations

* *Best Model*: LSTM (R² ≈ 0.965)
* *Production Strategy*: Ensemble (XGBoost + LSTM)
* *SARIMA & Linear Regression*: Useful as baselines

### Recommendation

Use a hybrid LSTM + XGBoost model for robust, high-accuracy day-ahead forecasting.

---

## 8. Future Work

* Include more weather/socioeconomic features
* Test Transformer-based models (e.g., TFT)
* Real-time forecasting pipeline
* Spatial-temporal clustering across cities

---

## Repository Structure


.
├── data/                         # Raw and cleaned datasets
├── notebooks/                   # Jupyter notebooks for EDA, modeling
├── models/                      # Trained models and metrics
├── app/                         # Flask backend and frontend interface
├── outputs/                     # Plots, exports, cluster summaries
├── weekly_summary.csv           # Aggregated trends per city
└── preprocessed_and_cleaned_data.csv


---

## Contact

For any questions or contributions, please contact:

* Muhammad Qasim
* Ayaan Khan
* Abu Bakr Nadeem
