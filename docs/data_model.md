# Data Model and Storage Design

## Overview
This project uses **MongoDB Atlas** as a persistent NoSQL storage layer for stock market time-series data.  
The data model is designed to support scalable ingestion, reproducible analytics, and distributed processing using **Apache Spark**.

To preserve data lineage and reproducibility, **raw ingested data is stored separately from derived and analytical outputs**.

---

## Database
**Name:** `Stocks`

---

## Collections

### `raw_prices`
- Stores immutable, source-of-truth market data ingested from external providers
- Includes both historical daily prices and near-real-time quote snapshots
- Each record is uniquely identified by `(symbol, timestamp, interval)`
- Serves as the authoritative input for all downstream processing

---

### `daily_agg`
- Stores daily aggregates and rolling statistics derived from raw data
- Includes returns, log returns, moving averages, and rolling volatility
- Computed using **MongoDB aggregation pipelines** and persisted for reuse

---

### `indicators`
- Stores technical indicators derived from daily data
- Includes RSI-14 values
- Computed using **MongoDB aggregation pipelines** and persisted separately from raw data

---

### Spark Output Collections
- **`spark_risk_return`**: Per-symbol annualized risk and return metrics
- **`spark_monthly_returns`**: Monthly aggregated returns per symbol
- These collections are written by **Apache Spark** and used for downstream visualization

---

## Indexing Strategy
A compound unique index on `(symbol, timestamp, interval)` is enforced on the `raw_prices` collection to ensure idempotent ingestion and prevent duplicate records.  
Additional indexes support efficient symbol- and time-based queries.

---

## Design Rationale
- **Persistent Storage:** Enables reproducibility and avoids reliance on in-memory computation  
- **Separation of Concerns:** Raw data, derived features, and Spark outputs are stored independently  
- **Scalability:** Supports multi-symbol and multi-year data growth without architectural changes  
- **Big Data Alignment:** Explicitly addresses volume and velocity dimensions  

Veracity considerations are not part of project scope. 

---
