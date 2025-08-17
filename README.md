# Autoscout scraper, cleaner and analysis

This folder contains tools and notebooks for scraping, cleaning and analysing Autoscout (as24) listings used in the EuropeVans project.

## Overview

- `as24_scraper.ipynb` — scraping pipeline and utilities: collects listing URLs, scrapes URLs for full listing details, stores results under `data/`. All of the above use Concurrent workers.
- `as24_clean.ipynb` — cleaning data: parses raw pages or exported tables into structured columns (price, mileage, power, dimensions, equipment), encodes categorical fields and produces cleaned parquet/csv under `data/`.
- `as24_analysis.ipynb` — analysis and modeling: loads cleaned data, trains or loads a CatBoost price model from `catboost_info/` and `models/`, evaluates predictions, and produces plots / diagnostics.

## Repository layout

- `as24_scraper.ipynb` — Jupyter notebook for scraping/autodata extraction.
- `as24_clean.ipynb` — Jupyter notebook for cleaning and feature engineering (StockCleaner pipeline).
- `as24_analysis.ipynb` — Jupyter notebook for analysis, feature inspection, model training and prediction.
- `Archive/` — raw exported datasets, historical scraped pages and intermediate parquet files.
- `data/` — cleaned datasets ready for modeling. (contains `.gitkeep` to keep folders tracked)
- `catboost_info/` — CatBoost training artifacts, logs and `.gitkeep`.
- `models/` — trained model binaries (`.cbm`) and `.gitkeep`.
- `auto24-api/` — helper package and utilities used by the scraper.

## Quick start

### Prerequisites

- Python 3.8+ (notebooks use typical data stack; CatBoost for model tasks)

### Scrape (notebook)

1. Open `as24_scraper.ipynb` and run cells to collect listings. Adjust target search filters inside the notebook.
2. Scraper writes raw outputs to `data/` (parquet).

### Clean / Feature engineering

1. Open `as24_clean.ipynb`.
2. Run the cleaning pipeline cells to parse raw HTML/rows into structured columns. The notebook creates dummies, numeric conversions and feature groups used for modeling.
3. Resulting cleaned datasets are saved to `data/` (`.parquet`, `.csv`).

### Modeling & Analysis

1. Open `as24_analysis.ipynb`.
2. The notebook loads cleaned data and the CatBoost model from `models/` / `catboost_info/` and/or trains a new one.
3. Use the provided evaluation and prediction cells. Ensure `features_cb` ordering matches the model when predicting.

## Notes and best practices

- `.gitignore` excludes `data/`, `catboost_info/`, `models/` and `Archive/` content; placeholder `.gitkeep` files are used to keep directories tracked.
- When predicting with CatBoost, make sure input columns are in the same order as the training `features_cb`. Fill any missing columns (from new datasets) with sensible defaults rather than zeros when appropriate.
- Consider comparing feature distributions between training and target datasets before predicting — distribution shift is a common source of bias.

## Next steps (suggestions)

- Add a `requirements.txt`
- Add unit tests for the cleaning functions and a small smoke test for prediction.
