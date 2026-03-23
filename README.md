# 🌤️ Weather Condition Predictor & Geo-Locator

A machine learning project that predicts weather conditions from meteorological and air quality data, and estimates geographic coordinates (latitude/longitude) from the same features.

---

## 📌 Overview

This project trains two separate models on a labeled weather dataset:

1. **Classifier** — Predicts `condition_text` (e.g., Sunny, Rainy, Cloudy) using a Random Forest Classifier.
2. **Regressor** — Predicts `latitude` and `longitude` from weather features using a Random Forest Regressor.

Both models use scikit-learn pipelines with preprocessing and hyperparameter tuning via `GridSearchCV`.

---

## 📁 Project Structure

```
weather-ml/
├── weather.xlsx               # Training dataset
├── weather_test_150.xlsx      # Test dataset (150 samples)
├── final_predictions.csv      # Output: predicted condition_text labels
├── notebook.ipynb             # Main Jupyter Notebook
└── README.md
```

---

## 🧠 Models

### 1. Weather Condition Classifier

| Property | Value |
|---|---|
| Target | `condition_text` (multi-class) |
| Algorithm | `RandomForestClassifier` |
| Encoding | `LabelEncoder` on target |
| Best Params | `n_estimators=200`, `max_depth=None`, `min_samples_leaf=1` |
| CV Accuracy | ~84.8% |
| Validation Accuracy | ~85.6% |

### 2. Geo-Coordinate Regressor

| Property | Value |
|---|---|
| Target | `latitude`, `longitude` (multi-output) |
| Algorithm | `RandomForestRegressor` |
| Best Params | `n_estimators=200`, `max_depth=30`, `min_samples_leaf=1` |
| Validation MSE | ~467.79 |

---

## 🔧 Features Used

The following columns are **dropped** before training:

```
latitude, longitude, timezone, condition_text, last_updated_epoch,
last_updated, country, location_name, sunrise, sunset,
moonrise, moonset, moon_illumination, moon_phase
```

Remaining features used for training include:

- **Meteorological**: temperature (°C/°F), wind speed/direction, pressure, precipitation, humidity, cloud cover, feels-like temp, visibility, UV index, gust speed
- **Air Quality**: CO, Ozone, NO₂, SO₂, PM2.5, PM10, US EPA Index, GB DEFRA Index

---

## ⚙️ Pipeline

Both models follow the same pipeline structure:

```
Input Features
     │
     ▼
ColumnTransformer
 ├── Numerical → StandardScaler
 └── Categorical → OneHotEncoder (handle_unknown='ignore')
     │
     ▼
RandomForest (Classifier / Regressor)
     │
     ▼
Predictions
```

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy scikit-learn openpyxl
```

### Run

1. Place `weather.xlsx` and `weather_test_150.xlsx` in `D:\codes\databases\` (or update the paths in the notebook).
2. Open and run `notebook.ipynb` in Jupyter.
3. Predictions will be saved to `D:\codes\databases\final_predictions.csv`.

---

## 📊 Output

- `final_predictions.csv` — Predicted weather condition labels for the 150 test samples.
- `final_pred_latlong` — DataFrame with predicted `latitude` and `longitude` (in-memory; can be exported similarly).

---

## 📝 Notes

- The dataset has some **rare classes** with very few samples, which triggers a scikit-learn warning during cross-validation (`least populated class has only 1 member`). This is expected behavior.
- Redundant unit columns (e.g., both `temperature_celsius` and `temperature_fahrenheit`) are retained — consider dropping one of each pair to reduce multicollinearity in future iterations.
- The geo-coordinate regressor's MSE of ~467 suggests limited predictive power for coordinates from weather data alone; this is expected given the non-deterministic relationship.

---

## 🛠️ Tech Stack

- **Language**: Python 3.13
- **Libraries**: `pandas`, `numpy`, `scikit-learn`, `seaborn`, `openpyxl`
- **Environment**: Jupyter Notebook
