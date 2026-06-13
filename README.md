# Walmart-Sales-Prediction-An-End-to-End-Analytics-Predictive-Modeling
An end-to-end data pipeline &amp; predictive modelling project utilizing SQLite3 and Random Forest to forecast Walmart weekly sales.

---

## Project Overview
This project implements an **end-to-end data analytics and predictive modelling pipeline** using a historical dataset of Walmart sales across 45 stores. The objective is to analyze the business drivers behind weekly sales and build a robust Machine Learning model capable of forecasting future sales based on temporal patterns and macroeconomic indicators.

## Tech Stack & Skill
* **Data Manipulation:** Python (`Pandas`, `NumPy`)

* **Database Analytics:** SQL (SQLite3, Aggregations, Data Quality Checks)

* **Data Visualization:** `Seaborn`, `Matplotlib` (Correlation matrix heatmap, time-series line plots, box plots, styled bar plots)

* **Machine Learning:** `Scikit-Learn` (`RandomForestRegressor`, `Time-Based Masking/Splitting`)

* **Model Evaluation:** Mean Absolute Error (MAE), R-squared (R2 Score), Feature Importance Analysis

---


## Data Architecture & Pipeline Workflow

### 1. Data Ingestion & Storage (SQLite3)
To simulate a production environment where data resides in a data warehouse rather than in flat files, the raw data is first ingested into a local SQLite3 database (`walmart_analytics.db`) using Python’s native database interface.

### 2. Macro-Level Data Quality Check (SQL)
Before pulling the data for statistical analysis, initial data sanity checks were executed purely through **SQL queries** directly on the database to preserve computational resources.

* Checked and confirmed **0 missing (NULL)** values across all critical operational and macroeconomic columns.
* Validated data integrity by confirming **0 duplicate records** for unique `Store` and `Date` combinations.

### 3. Micro-Level Transformation (Pandas)
Once structural integrity was verified, the dataset was read into a Pandas DataFrame. The `Date` column, initially stored as a string format(`DD-MM-YYYY`), was converted into a proper `datetime64` object to unlock time-series operations and eliminate data formatting anomalies.

---

## Business Insights & Exploratory Data Analysis (EDA)

<img width="1384" height="484" alt="walmart tren" src="https://github.com/user-attachments/assets/d48c0586-097b-4331-8efd-543e8799934f" />


#### Temporal Dynamics & Holiday Surges
Visual analysis of the timeline revealed heavy, predictable seasonality spikes towards the end of Q4 annually. This strongly corresponds to massive consumer spending during holiday weeks (Thanksgiving and Christmas), which was further validated by a boxplot analysis showing a significantly hugher distribution variace and right-skewed outliers during holiday periods.

#### Macroeconomic Factors Correlation

<img width="848" height="784" alt="walmart heatmap" src="https://github.com/user-attachments/assets/0e8bda3f-4a0a-4d1e-807f-13d860b7ba32" />

A correlation heatmap analysis revealed that direct linear correlations between individual macroeconomic factors (such as `CPI`, `Unemployment`, or `Temperature`) and `Weekly_Sales` were weak (ranging from -0.11 to 0.04).
* **Data-Driven Strategy**: This week linear relationship mathematically proved that traditional Linear Regressin would fail (*underfit*) to solve this problem. Consumer behavior is highly conditional and non-linear, demanding an ensemble tree-based algorithm capable of captuirng multi-variable interactions.

---

## Feature Engineering & Robust Validation
* **Feature Extraction:** To help the machine learning algorthm map recurring annual patterns, the `Date` object was decomposed into discrete temporal features: `Year`, `Month`, and `WeekOfYear`. The `WeekOfYear` feature acts as a strong proxy for capturing the exact timeframe of holiday buying surges.

* **Chronological Splitting (Time-Based Split):** To avoid severe data leakage, a standard random train-test split was intentionally avoided. Instead, the model was trained on historical data from **2010 to 2011** and validated against the unseen data of 2012. This simulate a real deployment scenario: predicting the future using the past.

---

## Model Performance & Evaluation

<img width="884" height="584" alt="walmart feature importance" src="https://github.com/user-attachments/assets/a8d4cc33-0b1b-47c1-9615-8325aa327820" />

A Random Forest Regressor was selected due to its exceptional ability to manage non-linear boundaries and handle categorical indicators like `Store` ID without artificial ordering bias.

The model was evaluated against the unseen 2012 test set, yielding the following results:

* **$R^2$ Score (Coefficient of Determination): 72.37%** Interpretation: The engineered features allow the model to explain approximately 72.4% of the massive fluctuations in Walmart's weekly sales.

* **Mean Absolute Error (MAE): $157,309.96** Interpretation: On average, the model's predictions deviate by roughly $157k. Given that Walmart's average weekly revenue per store hovers well into the millions, this margin of error provides highly actionable bounds for operational decision-making.

---

## Business Recommendations
### 1. Capitalize on Feature Importance
Our model analysis reveals that `Store` (Store ID) is the overwhelmingly dominant predictor, accounting for over 61% of the model's decision-making power. This is followed by macroeconomic indicators, specifically CPI (18.3%) and Unemployment (11.8%), which play a more significant role in long-term sales variance than immediate environmental factors like temperature or fuel prices. Interestingly, while visual trends show massive seasonal spikes during holidays, the model prioritizes the baseline differences between individual store locations and broader economic health (CPI/Unemployment) as the primary drivers for weekly revenue stability.

### 2. Proactive Supply Chain Tuning
Inventory pipelines for high-performing stores should be aggressively scaled up exactly 2-3 weeks prior to `WeekOfYear` 46-48 to handle the Thanksgiving rush smoothly without risking supply friction.
