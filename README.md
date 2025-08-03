# Energy Forecasting Using Machine Learning

## Objective
This project aims to forecast **Global Active Power** consumption using historical household energy usage data. By building regression models, we predict future energy usage based on time-series and weather-related features.

---

## Dataset Description
The dataset contains minute-level measurements of electrical power consumption in a single household over a period of several days.

- Original features: Voltage, Global Intensity, Sub-meterings, etc.
- Additional engineered features: lag values, rolling mean, rolling std, is_weekend, etc.
- Missing values were handled after applying lag/rolling operations.

---

## Data Preprocessing
- Dropped missing values resulting from rolling and lag features.
- Added time-based features: day, month, day of week, is_weekend.
- Created lag features (1, 2, 7 days) and rolling statistics (mean, std).
- Final dataset size after cleaning: `1391 rows × 15 features`.

---

## Models Used and Performance

| Model             | RMSE       | MAE        | R² Score |
|------------------|------------|------------|----------|
| Linear Regression| 1.6496e-15 | 1.3231e-15 | 1.0000   |
| Random Forest     | 0.0148     | 0.0117     | 0.9979   |
| XGBoost           | **0.0130** | **0.0102** | **0.9984** |

---

## Best Model
**XGBoost Regressor** achieved the highest performance with:

- **R² Score: 0.9984**
- **Lowest RMSE and MAE**
- Very close match between actual and predicted values in the plot.
<br><br>

> Note: Linear Regression showed perfect performance due to potential data leakage (e.g., lack of shuffling). Hence, it is not considered reliable for comparison.

---

## Visualizations
- Actual vs Predicted plots were created for each model.
- Clear alignment between predicted and real values confirms model quality.

---

## Requirements
Make sure to install dependencies before running the notebook (only if not using Google Colab):

```bash
pip install pandas numpy matplotlib seaborn scikit-learn statsmodels xgboost gdown
```

---

## Weather Data Integration

As an additional experiment, external **weather data** (daily temperature and relative humidity) was integrated using the [Open-Meteo API](https://open-meteo.com/).

### What was added:
- Fetched historical weather data based on the household location (latitude and longitude).
- Merged weather data by date with the original dataset.
- Included `temperature_2m_max` and `relative_humidity_2m_mean` as features.
- Retrained models using the updated dataset with weather features.

### Key Results:
- The best model remained **XGBoost**, achieving:
  - MSE: 1.15
  - MAE: 0.74
  - R² Score: 0.41
- Weather features contributed moderately according to feature importance analysis.

> This step helped understand whether weather data can improve energy forecasting accuracy.

---

## Uncertainty Estimation

As an additional experiment, I implemented **quantile regression** using `GradientBoostingRegressor` to estimate uncertainty in our energy consumption predictions.

### What was done:
- Trained three separate models to predict the **10th**, **50th** (median), and **90th** percentiles.
- Created a visualization showing the **prediction interval (80%)** as a shaded area between the 10th and 90th percentile predictions.
- Compared the median prediction (50th percentile) against actual values for accuracy evaluation.

### Key Insights:
- The prediction interval gives a **range of expected values**, helping quantify model **uncertainty**.
- This is useful in real-world scenarios where understanding **forecast confidence** is important (e.g., energy planning and risk management).
- The results showed that most actual values fall within the prediction interval, indicating reliable estimation.

> This step aiming to enhance the models interpretability and reliability.

---
