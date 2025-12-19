# Real Estate Profitability Forecaster

![Python](https://img.shields.io/badge/Python-3.12-blue.svg)
![Status](https://img.shields.io/badge/Status-Prototype-yellow.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## Project Overview

This project implements an end-to-end Machine Learning pipeline to analyze real estate profitability. By training models on historical property attributes, the system predicts fair market values for both sales and rentals to assist in investment decision-making.

The core objective is to calculate the **Break-Even Point (BEP)** for potential investments by combining the outputs of two distinct predictive models:
1.  **Purchase Price Predictor:** Estimates the fair market buy price based on property features.
2.  **Rental Price Predictor:** Estimates the potential monthly yield.

By combining these predictions, the system approximates the time required to recover the initial investment.

---

## Dataset

**Source:** Real Estate Market Sample
**Context:** The project utilizes two distinct datasets to train the respective regression models independently. These datasets represent a snapshot of the Spanish real estate market.

| Dataset | Sample Size | Target Variable | Description |
| :--- | :--- | :--- | :--- |
| **Sale Flats** | 5,847 rows | `Purchase Price` (€) | Training data for property valuation features. |
| **Rent Flats** | 8,502 rows | `Monthly Rent` (€) | Training data for estimating potential rental yields. |

> **Note:** The data has been anonymized for privacy and compliance reasons. The project focuses on the *methodology* of price estimation rather than real-time market monitoring.

---

## Methodology

To ensure robust predictions and minimize overfitting, several strategic decisions were made **throughout the development pipeline**:

### 1. Preprocessing & Outlier Detection
* **Luxury Filter:** Applied statistical filtering to remove extreme outliers (e.g., luxury properties) that could skew the model for standard investment analysis.

### 2. Exploratory Data Analysis (EDA)
* **Correlation Analysis:** Identified key drivers of price fluctuation.
* **Dimensionality Reduction:** Applied **PCA** (Principal Component Analysis) to reduce noise and collinearity.
* **Statistical Testing:** Used **ANOVA** to understand the variance impact of categorical features.
* **Explainability:** Integrated **SHAP** (SHapley Additive exPlanations) values to interpret feature importance globally and locally.

### 3. Modeling Strategy
We employed a variety of regression techniques to maximize accuracy:
* **Bagging Regressors**
* **Random Forest**
* **XGBoost** (Extreme Gradient Boosting)

---

## Model Evaluation

To rigorously evaluate the performance of our models, we utilized the following metrics:

* **R² (Coefficient of Determination):**
    * *Significance:* Indicates the proportion of the variance in the dependent variable (Price) that is predictable from the independent variables (Features). A higher score implies a better fit.
* **RMSE (Root Mean Square Error):**
    * *Significance:* Measures the average magnitude of the errors in Euros. Unlike MAE, RMSE penalizes larger errors more heavily, which is crucial in financial forecasting where large deviations carry higher risk.

### Results

We evaluated multiple regression algorithms to identify the best performer. The tables below summarize the performance on the **Test Set**.

### Purchase Price Prediction
*Target: Total Sale Price (€)*

| Model | R² Score | RMSE (€) | Status |
| :--- | :--- | :--- | :--- |
| **Bagging Regressor** | 0.016 | 26,678 | ❌ Discarded |
| **Random Forest** | 0.045 | 24,049 | ❌ Discarded |
| **XGBoost** | **0.443** | **17,556** | ✅ **Selected** |

> **Insight:** The purchase market shows higher variance and is likely influenced by external macroeconomic factors or sentimental values not present in the dataset, making it harder to model than rental prices.

### Rental Price Prediction
*Target: Monthly Rent (€)*

| Model | R² Score | RMSE (€) | Status |
| :--- | :--- | :--- | :--- |
| **XGBoost** | **0.809** | **413** | ✅ **Selected** |

> **Conclusion:** The rental model achieves high accuracy (R² > 0.80), indicating that monthly yield is strongly correlated with structural features (location, size, amenities).

---

## Technologies Used

* **Language:** Python 3.12
* **Data Manipulation:** Pandas, NumPy
* **Visualization:** Matplotlib, Seaborn
* **Machine Learning:** Scikit-learn, XGBoost
* **Model Interpretability:** SHAP
* **IDE:** VSCode

---

## Limitations

While the model performs robustly based on structural features, it is subject to the following constraints due to the nature of the data:

* **Lack of Temporal Granularity (Seasonality):** The dataset represents a static snapshot of the market. Consequently, the model cannot account for seasonal fluctuations in rental prices (e.g., peak season spikes in coastal areas or start-of-term demand in university districts). The predictions reflect an average baseline market value rather than a time-specific dynamic price.

---

## 🔮 Future Improvements

To enhance the project's complexity and deployment readiness, future iterations will include:

* **Ensemble Learning:** Implementing **Stacking** strategies to combine the strengths of multiple base estimators.
* **Mixture of Experts (MoE):** developing specialized sub-models for specific property types (e.g., Penthouses vs. Studios).
* **API Integration:** Wrapping the model in a FastAPI container to serve real-time predictions.

---

## ⚖️ Disclaimer

This project is for **educational and portfolio purposes only**. The dataset used is a static sample provided for analysis and does not reflect real-time market data. This tool is intended to demonstrate Machine Learning workflows (Data Cleaning, PCA, Modeling) and should not be used for actual financial investment advice.
