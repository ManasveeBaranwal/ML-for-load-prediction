So we have 6 different models for load predictions of india short term 2023 using data from 2017-18 to 2022.
📘 Model 1: region_rf_shape_scaled.py

Title: Region-wise Shape Forecasting using Random Forest
What It Does:
	•	Predicts shape (hourly profile) of load, not absolute values.
	•	Later scales these hourly predictions to match annual peak and energy targets using scaling method B (shape-preserving rescaling).

Key Components:
	•	Uses RandomForestRegressor.
	•	Features: calendar (hour, day, month), lag features (1–168 hours), daily max/sum, and annual constants.
	•	Data used: 2018–2022 as training, 2023 as testing.

Output:
	•	Metrics (RMSE, MAPE, R²) for each Indian region.
	•	CSVs for regional predictions and metrics.

⸻

📘 Model 2: region_xgb_shape_scaled.py

Title: Region-wise Shape Forecasting using XGBoost
What It Does:
	•	Same as Model 1 but replaces Random Forest with XGBoost.
	•	Predicts load shape, then scales it to meet annual totals.

Key Components:
	•	Uses XGBRegressor with hyperparameters (e.g., max_depth, learning_rate, n_estimators).
	•	Same features and methodology as Model 1.

Output:
	•	Regional load predictions and performance metrics for 2023 using XGBoost.

⸻

📘 Model 3: region_fft_features.py

Title: Region-wise Forecasting using FFT + Statistical Features (XGBoost)
What It Does:
	•	Enhances input features with FFT-derived features (first 3 harmonics), daily mean, std, skewness, kurtosis.
	•	Trains a separate model per region using these rich features.

Key Components:
	•	Adds fft_1, fft_2, fft_3 as time-frequency domain features.
	•	Learns better representation of oscillatory behavior in load curves.
	•	Evaluates importance using Permutation Importance.

Output:
	•	Better generalization across regions with lower error.
	•	Plots and saves importance scores.

⸻

📘 Model 4: multi_output_xgb_forecast.py

Title: Multi-output XGBoost Forecasting
What It Does:
	•	Forecasts load + features (mean_24, std_24, fft_1, fft_2, fft_3) jointly.
	•	Trains a single multi-target XGBoost model per region.

Key Components:
	•	Uses MultiOutputRegressor(XGBRegressor(...)).
	•	Targets include raw load and summary statistics.

Output:
	•	CSV with predictions for each of the 6 targets.
	•	Metrics table with RMSE, R² per output per region.

⸻

📘 Model 5: multi_output_rf_forecast.py

Title: Multi-output Random Forest Forecasting
What It Does:
	•	Same as Model 4, but using Random Forest instead of XGBoost.
	•	Predicts load + statistical + FFT features.

Key Components:
	•	Stronger model, more explainable, but showed some overfitting on training.

Output:
	•	Metrics for 6 outputs.
	•	CSV files for each region’s predictions.

⸻

📘 Model 6: forecast_2030_from_excel.py

Title: Iterative 2030 Forecasting with Random Forest
What It Does:
	•	Predicts 2030 hourly load region-wise by iteratively forecasting each hour using last 168 hours of history.
	•	Ensures compatibility with lag features and rolling features.

Key Components:
	•	Uses 2018–2022 as training.
	•	Initializes 2030 using CEA projected year_peak and year_energy.
	•	Rolling predictions for entire 2030 horizon.

Output:
	•	8760-hour prediction for each region.
	•	Comparison table with predicted vs CEA targets for 2030.
