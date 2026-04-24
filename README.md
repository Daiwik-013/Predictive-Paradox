# Predictive-Paradox
# Electricity Demand Forecasting

The aim of this project is to predict the next electricity demand on the national grid using previous demand data.

The raw dataset contained several inconsistencies. To ensure reliability:

- Duplicate entries were removed
- Missing values were handled appropriately (primarily filled with zeros for generation-related features)
- Extreme spikes and outliers in demand were controlled using percentile-based clipping (1st and 99th percentiles)

These steps helped in stabilizing the data while preserving its structure.

Since classical machine learning models do not inherently understand time, temporal patterns were manually encoded through feature engineering:

- **Time-based features**: hour of the day, day of the week, and month to capture seasonality and daily cycles
- **Lag features**:
  - `lag_1`: previous hour demand
  - `lag_24`: same hour previous day
  - `lag_168`: same hour previous week
- **Rolling feature**:
  - 24-hour moving average to capture recent trends

These features allow the model to learn short-term dependencies and recurring demand patterns.

---

## Model Selection
A **Random Forest Regressor** was chosen for this task due to its robustness and ability to model nonlinear relationships in tabular data without requiring extensive preprocessing.

---

## Training Strategy
To maintain proper validation:

- The dataset was sorted out chronologically
- A time-based split was used:
  - Training data: all of the data before 2023
  - Test data: data from 2023
- This ensures no future information leaks into the training process

## Evaluation
The model was evaluated using **Mean Absolute Percentage Error (MAPE)**.

- **Final MAPE:** ~5.7%

This indicates strong predictive performance for short-term demand forecasting.

## Key Insights
Feature importance analysis revealed that:

- **Recent demand (lag_1)** is the most influential factor
- **Daily patterns (lag_24)** significantly impact demand
- Time-of-day effects (hour feature) also play an important role

This confirms that electricity demand is highly dependent on short-term historical behavior and daily cycles.

## Design Decisions
To avoid misleading results:

- The current demand (`demand_mw`) and generation (`generation_mw`) variables were excluded from the feature set to prevent data leakage
- Weather and macroeconomic data were not included due to inconsistencies and their relatively lower impact on short-term (hourly) prediction

---

## Conclusion
The model successfully captures temporal patterns in electricity demand using engineered features. The results demonstrate that even classical machine learning approaches can perform effectively when appropriate feature engineering is applied.. 
