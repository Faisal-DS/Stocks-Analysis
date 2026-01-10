# Stock Market Analysis Using Big Data Storage and Processing

## 1. Project Overview
This project is developed as a semester project for **COMP-548DL – Big Data Management and Processing**, assigned by **Dr. Demetris Trihinas**.

The objective of the project is to design and implement an **end-to-end Big Data analytics pipeline** for financial time-series data. The focus of the work is on **Big Data storage, ingestion, management, and distributed processing**, while financial market data is used as a realistic and scalable application domain.

Rather than building financial trading or prediction models, the project demonstrates how modern Big Data technologies can be used to persistently store, process, and analyze continuously growing datasets in a reproducible and scalable manner.

---

## Quick Navigation
- [Project Scope and Dataset Plan](docs/scope.md)
- [Data Model and Indexing Strategy](docs/data_model.md)
- [Reproducibility Instructions](#10-reproducibility-instructions)

---

## 2. Big Data Challenges Addressed
The project explicitly addresses the following Big Data dimensions:

- **Volume**  
  Multi-year historical market data is persistently stored across multiple assets, resulting in a continuously growing time-series dataset.

- **Velocity**  
  Near-real-time market data is ingested periodically, simulating streaming-style data arrival and incremental dataset growth.

---

## 3. Dataset and Assumptions
- **Asset Universe**:  
  AAPL, MSFT, NVDA, AMZN, TSLA, APP, SMCI, SPY

- **Market**:  
  U.S. equity markets (NYSE and NASDAQ)

- **Time Range**:  
  Approximately three years of daily historical data, plus periodic near-real-time quote snapshots

- **Time Zone**:  
  All timestamps are normalized to UTC to ensure consistency across ingestion and processing stages

- **Assumptions**:
  - Data provided by external APIs is assumed to be structurally valid
  - The project does not attempt to validate or correct financial data quality (hence veracity dimension is not addressed)

---

## 4. System Architecture (Big Data Pipeline)
The project follows a modular Big Data architecture:

1. **Data Ingestion**
   - Batch ingestion of historical daily OHLCV data (OHLCV data stands for Open, High, Low, Close, and Volume, representing a financial asset's trading activity over a specific time period)
   - Periodic ingestion of near-real-time quote data (velocity simulation)

2. **Persistent Storage**
   - MongoDB Atlas is used as the central storage layer
   - Raw and derived datasets are stored persistently with enforced indexing and schema keys

3. **Distributed Processing**
   - Apache Spark (PySpark) is used to perform distributed analytics on persisted data
   - Spark reads from and writes back to MongoDB, acting as a scalable processing layer

4. **Visualization**
   - Analytical outputs are visualized to illustrate trends, volatility, and technical indicators via Jupyter Notebooks

---

## 5. Data Model and Storage Design
A MongoDB database named `Stocks` is used, with the following collections:

- `raw_prices` – Immutable raw market data (daily bars and quote snapshots)
- `daily_agg` – Derived daily aggregates and rolling statistics
- `indicators` – Technical indicators (e.g., RSI)
- `spark_risk_return` – Cross-sectional analytics produced by Apache Spark
- `spark_monthly_returns` – Monthly aggregated returns produced by Apache Spark

Compound unique indexes on `(symbol, timestamp, interval)` ensure idempotent ingestion, prevent duplicates, and support efficient querying.

---

## 6. Data Ingestion and Validation
- Historical daily data is ingested in batch mode
- Near-real-time quote data is ingested periodically to demonstrate velocity handling
- Idempotent upserts ensure safe re-execution of ingestion jobs
- Validation checks confirm:
  - Consistent record counts across assets
  - No duplicate records
  - Financial sanity of stored values

---

## 7. Distributed Processing with Apache Spark
Apache Spark is used to demonstrate distributed analytics on persistently stored data.

Spark performs:
- Joins across MongoDB collections
- Per-symbol risk and return summarization
- Monthly return aggregation using window functions

All Spark results are written back to MongoDB Atlas, ensuring reproducibility and avoiding transient, in-memory-only computation. Spark is used exclusively for scalable data processing and analytics, while visualization is treated as a downstream, non-Big-Data concern.

---

## 8. Analytics and Insights
The pipeline enables analytics such as:
- Risk vs. return comparison across assets
- Price trends with moving averages
- Rolling volatility analysis
- RSI-based momentum interpretation

These insights are derived from Big Data processing rather than ad-hoc, notebook-only analysis.

---

## 9. Proposal Alignment and Deviations
The project remains aligned with the original proposal in terms of objectives and scope. Minor deviations include the use of alternative data sources for historical data due to API limitations. These changes were made to ensure reliable ingestion while preserving the Big Data focus of the project.

---

## 10. Reproducibility Instructions

This project is fully reproducible without modifying any code.

### Prerequisites
- Python 3.10+
- Apache Spark 3.x
- Java 8 or 11
- MongoDB Atlas account
- Alpha Vantage API key

### Steps to Reproduce
1. Clone the repository
2. Create a `.env` file using `.env.example` as a template and provide:
   - MongoDB Atlas connection string
   - Alpha Vantage API key
3. Install dependencies:
   ```bash
   pip install -r requirements.txt


### Notebooks
- **Step3_Ingestion.ipynb**  
  Batch and velocity data ingestion with idempotent writes to MongoDB Atlas

- **Step6_Distributed_Analytics.ipynb**  
  Distributed analytics using Apache Spark on persisted datasets

- **Step7_Visualization.ipynb**  
  Visualization of persisted analytics results (read-only, visualization layer)

---

## 11. Project Video
A short demonstration video illustrating the system architecture, data flow, and analytical outputs will be published on YouTube.

*(Link to be added)*

---

## Repository
https://github.com/Faisal-DS/Stocks-Analysis
