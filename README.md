# Agmarknet Price Data: Acquisition, Preprocessing & Exploratory Analysis

## Overview

This project demonstrates an end-to-end data pipeline for agribusiness analytics, using real-time daily mandi price data sourced from **Agmarknet** (India's government agricultural market price portal) via **data.gov.in**. The raw dataset comprises 9,603 records spanning 23 states, 710 markets, and 176 commodities for a single-day snapshot, capturing minimum, maximum, and modal prices per market listing.

The project covers two stages. First, a **data preprocessing** pipeline addresses real-world quality issues — inconsistent state-name spellings, a mixed-semantics grading field, commodity-specific price outliers, suspected unit-of-entry errors, and high min-max spread anomalies — using targeted, justified interventions rather than blanket deletion. Second, an **exploratory data analysis (EDA)** stage examines the cleaned dataset through univariate and bivariate statistics and five visualizations, surfacing patterns relevant to agribusiness decision-making such as price volatility behavior, commodity-level price instability, and regional price variation.

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
│   └── agmarknet_eda_walkthrough.ipynb
└── reports/
    ├── agribusiness_data_preprocessing_report.docx
    └── agribusiness_eda_report.docx
```

## Stage 1: Data quality issues identified

- Extreme price outliers and suspected unit-of-entry errors (per-kg values recorded in a per-quintal field)
- Inconsistent state-name spellings (for example, "Keralam" vs "Kerala")
- Inconsistent commodity and variety taxonomy across records
- High min-max price spread anomalies within single-day listings
- A grading field that mixes quality grade, size grade, and market-type labels under one column

## Stage 1: Preprocessing steps applied

1. **Categorical standardization** — state names mapped to canonical spellings, all text fields stripped of whitespace
2. **Grade taxonomy normalization** — the raw grade field mapped into Quality Grade, Size Grade, and Market Type categories
3. **Logical validity filtering** — records with Min > Max, or Modal price outside the Min-Max range, checked and removed
4. **Per-commodity IQR outlier detection** — outlier bounds computed separately for each commodity rather than globally, since price scales vary widely across commodities
5. **Spread-ratio and unit-error flagging** — records with a Max/Min ratio above 10x, or a modal price suspiciously below a commodity's typical price, flagged rather than auto-corrected
6. **Duplicate assessment** — exact duplicates dropped; records differing only by grade retained as legitimate multi-grade listings
7. **Derived fields** — price range and within-commodity normalized modal price added for downstream analysis

## Stage 2: Exploratory data analysis

- **Univariate analysis** — descriptive statistics (mean, median, standard deviation, quartiles, skewness, coefficient of variation) for all numeric price variables, and frequency summaries for categorical variables
- **Bivariate analysis** — a Pearson correlation matrix across numeric variables, and grouped price statistics by state, commodity, and grade category
- **Visualization** — a histogram of price distribution, a box plot of price spread by commodity, a scatter plot of Min vs Max price colored by outlier status, a bar chart of mean price by state, and a correlation heatmap
- **Key findings** — price volatility (Spread_Ratio) is statistically independent of price level; certain commodities (notably Green Chilli) show disproportionately wide price spread and outlier concentration; the dataset is geographically concentrated, with one state accounting for roughly 70% of records; and mean price differences between states with comparable commodity mixes point to a possible regional arbitrage signal

## How to run

1. Clone the repository
2. Install dependencies: `pip install pandas numpy matplotlib`
3. Run `notebooks/agmarknet_data_cleaning_walkthrough.ipynb` first to reproduce the cleaning pipeline
4. Run `notebooks/agmarknet_eda_walkthrough.ipynb` to reproduce the statistical analysis and charts

## Outputs

- `data/processed/agmarknet_cleaned.csv` — cleaned dataset with quality flags (Price_Outlier_Flag, Spread_Anomaly_Flag, Suspected_Unit_Error) and derived fields (Price_Range, Modal_Price_Normalized)
- `reports/agribusiness_data_preprocessing_report.docx` — full documentation of the acquisition, assessment, cleaning strategy, and impact analysis
- `reports/agribusiness_eda_report.docx` — full documentation of the exploratory data analysis, including statistical summaries, visualizations, and agribusiness-relevant insights
