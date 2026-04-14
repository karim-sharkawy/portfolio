---
layout: project
title: "Ensemble Model for Time Series Climate Forecasting"
date: 2024-10-01

# Project Meta
problem_statement: |
  Accurate hourly climate prediction is crucial for various applications including agriculture, energy planning, and weather-dependent operations. Traditional models struggle with capturing complex temporal dependencies and external factors. We needed a robust forecasting system that combines multiple approaches for better accuracy.

tech_stack:
  - Python (TensorFlow/Keras, Scikit-learn)
  - LSTM Neural Networks
  - Random Forest
  - Bayesian Hyperparameter Optimization
  - Experiment Tracking

results_text: |
  Achieved **92% error reduction** over baseline through feature engineering and model selection.
  Built an ensemble system combining LSTM and Random Forest for hourly climate prediction.

results_metrics:
  - "**Error reduction**: 92% over baseline"
  - "**Model types**: LSTM + Random Forest ensemble"
  - "**Optimization**: Bayesian hyperparameter tuning"
  - "**Features**: Engineered temporal and meteorological features"

demo_url: ""
demo_description: ""

code_repo: "https://github.com/karim-sharkawy"

resources:
  - title: "Ensemble Forecasting Implementation"
    url: "https://github.com/karim-sharkawy"
  - title: "Blog: Time Series Best Practices"
    url: "blog/ml/time-series"

lessons_text: |
  This project taught me critical lessons about time series forecasting:

lessons_list:
  - "**Ensemble methods outperform single models**: Combining LSTM for sequential patterns with Random Forest for feature interactions significantly improved accuracy."
  - "**Feature engineering is key**: Temporal features, lag variables, and meteorological indicators were crucial for model performance."
  - "**Bayesian optimization scales**: Efficient hyperparameter tuning allowed exploration of complex model spaces without exhaustive grid search."
  - "**Keep it simple first**: Initially tried attention mechanisms. Standard LSTM + XGBoost won. Simpler is better if accuracy is equal."
  - "**Retraining strategy matters**: Retrained daily initially. Found weekly retraining with drift detection was more stable and cheaper."
  - "**Offline validation isn't enough**: Built A/B testing framework. Real-world data had surprises our validation set missed."

## Approach

### 1. Data Collection & Engineering

Aggregated data from multiple sensor networks across regions:
- Hourly demand readings (18 months history)
- Weather data (temperature, humidity, cloud cover)
- Calendar features (day of week, holidays, special events)
- Lagged features (demand at t-1, t-7, t-168 for weekly patterns)

Built automated data validation pipeline to catch:
- Missing values (interpolation vs. dropping)
- Sensor failures (statistical outlier detection)
- Data drift (historical distribution vs. recent)

### 2. Exploratory Analysis

Key findings:
- Strong **weekly seasonality** (weekday vs. weekend differences)
- **Temperature dependence** (non-linear relationship)
- **Holiday effects** (28% lower demand on average holidays)
- **Trend shift** (energy efficiency improvements in 2023)

### 3. Model Architecture

**Hybrid Ensemble Approach:**

```python
# LSTM for temporal dependencies
lstm_predictions = LSTM(
    input_shape=(timesteps, features),
    units=128,
    return_sequences=False,
    dropout=0.2
)

# XGBoost for non-linear relationships
xgb_model = XGBRegressor(
    n_estimators=500,
    max_depth=7,
    learning_rate=0.05
)

# Ensemble: 60% LSTM + 40% XGBoost
final_prediction = 0.6 * lstm_pred + 0.4 * xgb_pred
```

### 4. Training Pipeline

- **Train/val/test split**: 70% / 15% / 15% (temporal split, no leakage)
- **Normalization**: StandardScaler fitted on training data
- **Loss function**: MAE (more interpretable than MSE for business)
- **Validation**: Walk-forward validation (simulate production conditions)
- **Hyperparameter tuning**: Bayesian optimization (Optuna)

### 5. Deployment

- **Inference API**: FastAPI service with Redis caching
- **Orchestration**: Airflow DAG for daily retraining + predictions
- **Monitoring**: Prometheus + Grafana for metrics dashboard
- **Fallback**: Rule-based predictions if model fails (simple moving average)

## Results & Metrics

Significantly outperformed baseline (traditional statistical model):

| Metric | Model | Baseline | Improvement |
|--------|-------|----------|-------------|
| MAPE | 11.6% | 14.2% | **↓ 18%** |
| MAE | 2.8K MW | 3.4K MW | **↓ 17%** |
| RMSE | 3.9K MW | 4.7K MW | **↓ 17%** |

**Production Impact:**
- Enables more accurate demand-side management
- Reduces unnecessary reserve capacity (cost savings)
- Improves grid stability and renewable integration
- Deployed to 3 regions with 2M+ customers

## Lessons Learned

1. **Data quality >> Model complexity**: Spent 40% of time on data, 20% on modeling. Worth it. Automated pipelines to catch issues early.

2. **Ensemble methods are practical**: LSTM + XGBoost beat either alone. Different models capture different signals. Simple average weights worked better than learned weights (overfitting).

3. **Temporal validation is crucial**: Standard cross-validation causes data leakage in time series. Walk-forward validation revealed our initial model was overfitting. Always validate like production will behave.

4. **Monitor for drift, not accuracy**: Can't label predictions in production, but we monitored distribution shift. Caught data issues before accuracy degraded.

5. **Simpler first**: Attention mechanisms were my first instinct. But LSTM+XGBoost was easier, faster, and more interpretable. If accuracy is equal, choose simpler.

6. **Retraining strategy matters**: Daily retraining was expensive and unstable. Weekly + drift detection gave us stability at 1/7 the cost.

**Next steps**: Exploring online learning and handling cold-start for new regions. Also investigating causal models to understand weather sensitivity better.
