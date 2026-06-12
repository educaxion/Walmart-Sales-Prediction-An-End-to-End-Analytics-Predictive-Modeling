# Walmart-Sales-Prediction-An-End-to-End-Analytics-Predictive-Modeling
An end-to-end data pipeline &amp; predictive modelling project utilizing SQLite3 and Random Forest to forecast Walmart weekly sales.

---

## Project Overview
This project implements an **end-to-end data analytics and predictive modelling pipeline** using a historical dataset of Walmart sales across 45 stores. The objective is to analyze the business drivers behind weekly sales and build a robust Machine Learning model capable of forecasting future sales based on temporal patterns and macroeconomic indicators.


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
#### Temporal Dynamics & Holiday Surges
Visual analysis of the timeline revealed heavy, predictable seasonality spikes towards the end of Q4 annually. This strongly corresponds to massive consumer spending during holiday weeks (Thanksgiving and Christmas), which was further validated by a boxplot analysis showing a significantly hugher distribution variace and right-skewed outliers during holiday periods.

#### Microeconomic Factors Correlation
A correlation heatmap analysis revealed that direct linear correlations between individual macroeconomic factors (such as `CPI`, `Unemployment`, or `Temperature`) and `Weekly_Sales` were weak (ranging from -0.11 to 0.04).
* **Data-Driven Strategy**: This week linear relationship mathematically proved that traditional Linear Regressin would fail (*underfit*) to solve this problem. Consumer behavior is highly conditional and non-linear, demanding an ensemble tree-based algorithm capable of captuirng multi-variable interactions.

---

## Feature Engineering & Robust Validation
* **Feature Extraction:** To help the machine learning algorthm map recurring annual patterns, the `Date` object was decomposed into discrete temporal features: `Year`, `Month`, and `WeekOfYear`. The `WeekOfYear` feature acts as a strong proxy for capturing the exact timeframe of holiday buying surges.

* **Chronological Splitting (Time-Based Split):** To avoid severe data leakage, a standard random train-test split was intentionally avoided. Instead, the model was trained on historical data from **2010 to 2011** and validated against the unseen data of 2012. This simulate a real deployment scenario: predicting the future using the past.

---
