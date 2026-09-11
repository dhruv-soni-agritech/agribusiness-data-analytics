# Agmarknet Price Data: Acquisition, Preprocessing, Analysis & Predictive Modelling
 
## Overview
 
This project demonstrates an end-to-end data pipeline for agribusiness analytics, using real-time daily mandi price data sourced from **Agmarknet** (India's government agricultural market price portal) via **data.gov.in**. The raw dataset comprises 9,603 records spanning 23 states, 710 markets, and 176 commodities for a single-day snapshot, capturing minimum, maximum, and modal prices per market listing.
 
The project covers three core stages plus a supplementary extension. First, a **data preprocessing** pipeline addresses real-world quality issues — inconsistent state-name spellings, a mixed-semantics grading field, commodity-specific price outliers, suspected unit-of-entry errors, and high min-max spread anomalies. Second, an **exploratory data analysis (EDA)** stage examines the cleaned dataset through univariate and bivariate statistics and five visualizations. Third, an **advanced predictive modelling** stage builds a cross-sectional price-benchmarking regression model, estimating expected commodity price from market context. A **supplementary analysis** extends the same approach to classify data-quality risk itself, identifying which states and markets are most prone to anomalous listings.
 
## Data source
 
- **Source:** Agmarknet, Directorate of Marketing & Inspection, Ministry of Agriculture and Farmers Welfare, Government of India
- **Access point:** data.gov.in
- **Snapshot date:** 23 August 2026
- **Granularity:** State, District, Market, Commodity, Variety, Grade, Arrival Date, Min/Max/Modal Price (per quintal)
## Repository structure
 
```
agmarknet-price-data-preprocessing/
├── README.md
├── data/
│   ├── raw/
│   │   └── agmarknet_raw_20260823.csv
│   └── processed/
│       └── agmarknet_cleaned.csv
├── notebooks/
│   ├── agmarknet_data_cleaning_walkthrough.ipynb
│   ├── agmarknet_eda_walkthrough.ipynb
│   ├── agmarknet_predictive_modelling_walkthrough.ipynb
│   └── agmarknet_outlier_classification_walkthrough.ipynb
└── reports/
    ├── Strategic_Agribusiness_Data_Analysis_Plan.docx
    ├── Strategic_Agribusiness_Data_Analysis_Plan.pdf
    ├── agribusiness_data_preprocessing_report.docx
    ├── agribusiness_eda_report.docx
    ├── agribusiness_predictive_modelling_report.docx
    └── agribusiness_outlier_classification_report.docx
```
 
`Strategic_Agribusiness_Data_Analysis_Plan` (available as both .docx and .pdf) is the overarching plan for this project, setting out the objectives and phased approach that the four stages below implement.
 
## Stage 1: Data preprocessing
 
**Quality issues identified:** extreme price outliers and suspected unit-of-entry errors, inconsistent state-name spellings, inconsistent commodity/variety taxonomy, high min-max spread anomalies, and a grading field mixing quality, size, and market-type labels.
 
**Steps applied:** categorical standardization, grade taxonomy normalization, logical validity filtering, per-commodity IQR outlier detection, spread-ratio and unit-error flagging, duplicate assessment, and derived fields (Price_Range, Modal_Price_Normalized).
 
## Stage 2: Exploratory data analysis
 
Univariate statistics (mean, median, standard deviation, skewness, coefficient of variation) and bivariate analysis (correlation matrix, grouped statistics by state/commodity/grade), supported by five visualizations. Key findings: price volatility (Spread_Ratio) is statistically independent of price level; certain commodities (notably Green Chilli) show disproportionately wide price spread; the dataset is geographically concentrated, with one state accounting for roughly 70% of records.
 
## Stage 3: Advanced predictive modelling
 
A cross-sectional price-benchmarking regression model (Linear Regression and Random Forest) predicts expected Modal Price from State, Commodity, and Grade_Category, deliberately excluding Min/Max/Price_Range/Spread_Ratio to avoid data leakage. The Random Forest model achieves an R-squared of 0.367, with Commodity identified as the dominant price driver (75% of feature importance). The report documents a methodological correction made during development: an initial category-grouping rule collapsed over half the data into an uninformative "Other" bucket, suppressing most of Commodity's real predictive signal; correcting this rule improved R-squared substantially from an initial 0.094.
 
## Supplementary: Outlier classification as a data-quality diagnostic
 
Using the same feature set, a classifier predicts whether a listing is a statistical price outlier rather than predicting price itself. This surfaces a contrasting pattern: State, not Commodity, is the dominant predictor of outlier status, and outlier rates vary sharply by market (some markets show rates 8-9x the dataset average), pointing toward geography-driven data-quality risk rather than commodity-specific volatility. This stage also documents the class-imbalance problem explicitly (only 3.58% of records are outliers), contrasting a naive classifier (high accuracy, near-zero recall) against a class-balanced one.
 
## How to run
 
1. Clone the repository
2. Install dependencies: `pip install pandas numpy matplotlib scikit-learn`
3. Run the notebooks in `notebooks/` in order: data cleaning, then EDA, then predictive modelling, then outlier classification
## Outputs
 
- `reports/Strategic_Agribusiness_Data_Analysis_Plan.docx` / `.pdf` — the overarching project plan and objectives
- `data/processed/agmarknet_cleaned.csv` — cleaned dataset with quality flags and derived fields
- `reports/agribusiness_data_preprocessing_report.docx` — acquisition, assessment, cleaning strategy, and impact analysis
- `reports/agribusiness_eda_report.docx` — statistical summaries, visualizations, and agribusiness-relevant insights
- `reports/agribusiness_predictive_modelling_report.docx` — regression model design, evaluation, and business recommendations
- `reports/agribusiness_outlier_classification_report.docx` — supplementary data-quality diagnostic analysis
