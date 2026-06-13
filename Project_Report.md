# Project 10: Quality Prediction in Mining Process

**Company:** UCT  
**Domain:** Data Science / Industrial Machine Learning  
**Dataset:** Flotation Plant Process Data (March–September 2017)  

---

## 1. Background

Mining operations rely on flotation plants to separate valuable minerals from ore. The process involves injecting air and chemicals into a slurry, creating froth that carries iron concentrate while impurities sink. A critical quality metric is the % Silica (impurity) in the final iron ore concentrate. Currently, silica is measured in a lab on an hourly basis. If engineers could predict silica levels in advance using process sensor data, they could take corrective actions — adjusting air flow, reagent dosing, or feed rates — before quality degrades. This reduces waste sent to tailings, improves recovery rates, and lowers environmental impact.

---

## 2. Problem Statement

The objective is to predict **% Silica Concentrate** in iron ore using real-time process measurements from a flotation plant. The project addresses three specific questions:

1. Is it possible to predict % Silica every minute?
2. How many hours ahead can we predict % Silica?
3. Can we predict % Silica without using the % Iron Concentrate column (as they are highly correlated)?

---

## 3. Design / Approach

1. **Data Loading & Cleaning:**  
   - Loaded 737,453 rows, 33 columns.  
   - Fixed European comma decimal separators (e.g., `4,7047` → `4.7047`).  
   - Parsed datetime index (March–September 2017).  
   - Confirmed effective hourly sampling rate despite multiple simultaneous sensor readings.

2. **Exploratory Data Analysis:**  
   - Plotted % Silica over time: fluctuates between ~0.5% and ~5.5%.  
   - Computed correlations: % Iron Concentrate strongly negatively correlated with silica (r = -0.801).  
   - Identified key secondary variables: % Silica Feed, Amina Flow, Ore Pulp Density.

3. **Feature Engineering:**  
   - Created lag features for % Silica and % Iron Concentrate at 1h, 2h, 3h, 6h, 12h.  
   - This enables multi-step-ahead forecasting.  
   - Dropped rows with NaN values from lagging.

4. **Train/Test Split:**  
   - Chronological split (80% train: March–July, 20% test: July–September) to respect time ordering and prevent future data leakage.

5. **Modeling:**  
   - **With Iron Concentrate:** Linear Regression (baseline) and Random Forest.  
   - **Without Iron Concentrate:** Same models to test predictive power of remaining process variables.

6. **Evaluation:**  
   - Metrics: RMSE and R².  
   - Feature importance analysis (Random Forest).  
   - Predicted vs Actual scatter plots.

---

## 4. Implementation Details

- **Language:** Python 3  
- **Libraries:** Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn  
- **Environment:** Google Colab  
- **Dataset:** Flotation plant process data (24 columns: quality measures, process variables, lab results)  
- **Data preprocessing:** Comma-to-dot decimal fix, datetime indexing, lag feature creation  
- **Sampling:** Random Forest optimized (50 trees, max_depth=8, 50% sample per tree) to handle large dataset  

---

## 5. Results

| Model                | RMSE    | R²      |
|----------------------|---------|---------|
| LR with Iron         | 0.6974  | 0.6292  |
| RF with Iron         | 0.7062  | 0.6198  |
| LR without Iron      | 1.1476  | -0.0041 |
| RF without Iron      | 1.2338  | -0.1600 |

- **Best model:** Linear Regression with Iron Concentrate (RMSE: 0.70, R²: 0.63).  
- Random Forest did not outperform Linear Regression, indicating the relationship is largely linear.  
- **Without Iron Concentrate, R² drops to near zero or negative** — the model is worse than predicting the mean. Iron Concentrate is an essential predictor.  
- **Top features (Random Forest importance):**  
  - % Iron Concentrate: 72.0%  
  - % Iron Concentrate lag 2h: 4.8%  
  - % Iron Concentrate lag 1h: 4.2%  
  - % Silica Feed: 2.2%  
  - % Iron Feed: 1.6%  

---

## 6. Answers to Key Questions

**Q1: Is it possible to predict % Silica every minute?**  
No. The data is effectively sampled hourly. Minute-level prediction would require higher-frequency sensor data or interpolation that introduces assumptions and reduces reliability.

**Q2: How many hours ahead can we predict?**  
With lag features up to 12 hours, predictions are possible up to 12 hours ahead. Longer horizons would require additional lag engineering and likely increase error.

**Q3: Can we predict without % Iron Concentrate?**  
No. Removing Iron Concentrate increases RMSE by 0.45 and drops R² from 0.63 to -0.004. The high negative correlation (r = -0.801) makes Iron Concentrate essential — other process variables alone provide negligible predictive power.

---

## 7. Learnings

- **Domain understanding matters:** The near-perfect inverse relationship between Iron and Silica is obvious to a metallurgist but was discovered through correlation analysis — a reminder to always explore data before modeling.  
- **Simple models can win:** Linear Regression outperformed Random Forest, reinforcing that complexity should be justified, not assumed.  
- **Feature engineering is critical:** Lag features transformed a static regression problem into a time-series forecasting one, enabling multi-hour-ahead predictions.  
- **Data quality issues are real:** The European decimal separator format (commas) is common in industrial data. Handling it early prevents silent errors.  
- **Honesty in results:** Reporting that models fail without Iron Concentrate is as valuable as reporting successes — it saves the plant from deploying a useless predictor.

---

**Repository:** https://github.com/zom1xo/mining-quality-prediction  
**Notebook:** `Project10_Mining_Quality.ipynb`
