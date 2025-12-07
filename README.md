# Daily Solar Power Forecasting using Machine Learning

## Project Overview

This project implements an end-to-end data analysis pipeline to forecast the **daily mean solar power output ($kW/hr$)** for a specific Photovoltaic (PV) system. The solution integrates disparate, publicly available time-series datasets, applies robust feature engineering, and utilizes advanced ensemble machine learning models to generate a high-accuracy, **1-day-ahead forecast**.

This work was developed as the final Student Choice Module for **ATMS 523: Weather and Climate Data Analytics**.

### Key Objectives

* **Data Integration:** Combine historical PV performance data (PVDAQ) with local meteorological data (NREL NSRDB).
* **Feature Engineering:** Create necessary time-series features (**lags, seasonal harmonics**) for causal forecasting.
* **Model Building:** Train and compare advanced regression models, including Random Forest and XGBoost.
* **Validation:** Benchmark model performance against a **Naive (Persistence) Forecast Baseline**.
* **Interpretability:** Use **SHAP** (SHapley Additive exPlanations) to interpret model predictions and feature importance.

***

## Data Sources

The project leverages two primary open data repositories:

| Dataset | Type | Source Access Method | Key Variables Used |
| :--- | :--- | :--- | :--- |
| **PVDAQ Data** (System ID 11487) | Historical PV Power | AWS S3 via `s3fs` | `Total_kWh`, `Power_Mean` |
| **NREL NSRDB** | Meteorological Data | NREL API Client | GHI, Clearsky GHI, Temperature, Wind Speed, Cloud Type |

Both hourly datasets were resampled to daily means and carefully aligned on a timestamp index to create the final unified feature set.

***

## Methodology & Results

The analysis is performed within the provided **`Project_Code.ipynb`** Jupyter notebook.

### Model Evaluation and Comparison

The final models were validated using a **true time-series split** (80% training, 20% testing). The XGBoost Regressor provided the best performance metrics:

| Model | RMSE (Root Mean Squared Error) | R² (Coefficient of Determination) |
| :--- | :--- | :--- |
| **Naive Forecast (Baseline)** | 0.2053 | 0.6490 |
| Random Forest Regressor | 0.1707 | 0.7575 |
| **XGBoost Regressor (Best)** | **0.1655** | **0.7719** |

### Key Findings

* **Model Skill:** The **XGBoost Regressor** significantly improved upon the Naive Forecast, reducing the prediction error (RMSE) by approximately **19.5%**.
* **Dominant Feature:** SHAP analysis confirmed that the **Global Horizontal Irradiance (GHI)** is the single most critical factor for predicting mean power output.

***

## Running the Code

### Requirements

This project requires Python 3.8+ and the following libraries. Install them using `pip`:

```bash
pip install pandas s3fs numpy scikit-learn matplotlib xgboost shap
'''

### Execution Steps

1.  **Clone the Repository** (if applicable):
    ```bash
    git clone https://github.com/jbb-illini/ATMS-523-M8.git
    cd solar-forecasting
    ```

2.  **NREL API Key Setup:**
    * Obtain a free API key and register an email with NREL's website.
    * Open the **`Project_Code.ipynb`** notebook.
    * Edit the cell containing the API credentials (referred to as **Cell 6** in the notebook guide) to insert your personal `API_KEY` and `EMAIL`.

3.  **Run the Notebook:**
    * Execute all cells in the **`Project_Code.ipynb`** file sequentially. The notebook handles all steps from data acquisition and cleaning through model training, evaluation, and visualization.

