# Mining Quality Prediction

Predicting % Silica impurity in iron ore concentrate using flotation plant process data. Built during UCT data science internship.

## Overview
- **Problem:** Predict silica levels to enable proactive process control in a mining flotation plant.
- **Dataset:** 737,453 rows, 33 columns (March–September 2017).
- **Approach:** EDA → Lag feature engineering → Linear Regression baseline → Random Forest → Model comparison with/without key predictor.

## Results
| Model | RMSE | R² |
|-------|------|-----|
| Linear Regression (with Iron) | 0.697 | 0.629 |
| Random Forest (with Iron) | 0.706 | 0.620 |
| Without Iron Concentrate | 1.148–1.234 | ~0 |

## Key Findings
- % Iron Concentrate is the dominant predictor (r = -0.801, 72% feature importance).
- Without Iron Concentrate, predictive power vanishes.
- Predictions possible up to 12 hours ahead.
- Data is hourly; minute-level prediction not feasible.

## Files
- `Project10_Mining_Quality.ipynb` — Full notebook.
- `Project_Report.md` — Detailed report with methodology, results, and learnings.

## Tools
Python, Pandas, Scikit-learn, Matplotlib, Seaborn, Google Colab

## Author
[GitHub](https://github.com/zom1xo)
