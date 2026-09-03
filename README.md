Agmarknet Price Data Acquisition & Preprocessing
Overview

This project demonstrates an end-to-end data acquisition and preprocessing pipeline for agribusiness analytics, using real-time daily mandi price data sourced from Agmarknet (India's government agricultural market price portal) via data.gov.in. The raw dataset comprises 9,603 records spanning 23 states, 710 markets, and 176 commodities for a single-day snapshot, capturing minimum, maximum, and modal prices per market listing.

The preprocessing pipeline addresses five categories of real-world data quality issues: inconsistent state-name spellings, a mixed-semantics grading field, commodity-specific price outliers, suspected unit-of-entry errors (per-kilogram values entered where per-quintal was expected), and high min-max price spread anomalies. Rather than applying blanket deletion, the strategy favors targeted, justified interventions — categorical standardization, a controlled grading taxonomy, and per-commodity IQR-based outlier flagging (chosen over z-score methods for robustness to the right-skewed distribution typical of commodity prices).

The result is a cleaned, analysis-ready dataset with explicit quality flags preserved rather than discarded, allowing downstream analysis to selectively include or exclude anomalous records depending on whether the goal is central-tendency estimation or volatility analysis.

Data source
Source: Agmarknet, Directorate of Marketing & Inspection, Ministry of Agriculture and Farmers Welfare, Government of India
Access point: data.gov.in
Snapshot date: 23 August 2026
Granularity: State, District, Market, Commodity, Variety, Grade, Arrival Date, Min/Max/Modal Price (per quintal)
Repository structure
agmarknet-price-data-preprocessing/
├── README.md
├── data/
│   ├── raw/
│   │   └── agmarknet_raw_20260823.csv
│   └── processed/
│       └── agmarknet_cleaned.csv
├── notebooks/
│   └── agmarknet_data_cleaning_walkthrough.ipynb
└── reports/
    └── agribusiness_data_preprocessing_report.docx
Data quality issues identified
Extreme price outliers and suspected unit-of-entry errors (per-kg values recorded in a per-quintal field)
Inconsistent state-name spellings (for example, "Keralam" vs "Kerala")
Inconsistent commodity and variety taxonomy across records
High min-max price spread anomalies within single-day listings
A grading field that mixes quality grade, size grade, and market-type labels under one column
Preprocessing steps applied
Categorical standardization — state names mapped to canonical spellings, all text fields stripped of whitespace
Grade taxonomy normalization — the raw grade field mapped into Quality Grade, Size Grade, and Market Type categories
Logical validity filtering — records with Min > Max, or Modal price outside the Min-Max range, checked and removed
Per-commodity IQR outlier detection — outlier bounds computed separately for each commodity rather than globally, since price scales vary widely across commodities
Spread-ratio and unit-error flagging — records with a Max/Min ratio above 10x, or a modal price suspiciously below a commodity's typical price, flagged rather than auto-corrected
Duplicate assessment — exact duplicates dropped; records differing only by grade retained as legitimate multi-grade listings
Derived fields — price range and within-commodity normalized modal price added for downstream analysis
