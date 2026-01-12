# Project Scope & Dataset Plan

## Asset Universe
The project analyzes a fixed universe of U.S. equity instruments:

**Symbols (8):**  
AAPL, MSFT, NVDA, AMZN, TSLA, APP, SMCI, SPY

These assets were selected to provide diversity in market capitalization, volatility, and trading behavior while remaining manageable.

---

## Data Sources

### Historical Data (Batch Ingestion)
- **Primary Provider:** Stooq
- **Data Type:** Daily OHLCV (Open, High, Low, Close, Volume)
- **Format:** CSV (programmatically ingested)
- **Rationale:**  
  Stooq provides stable and freely accessible historical market data suitable for reproducible batch ingestion and long-horizon analysis.

### Near-Real-Time Data (Velocity Simulation)
- **Provider:** Alpha Vantage API
- **Endpoint:** GLOBAL_QUOTE
- **Data Type:** Latest quote snapshot per symbol
- **Ingestion Pattern:** Periodic polling
- **Rationale:**  
  Periodic quote ingestion simulates data velocity without introducing the operational complexity of a full streaming infrastructure.

---

## Datasets

### Daily Historical Prices
- **Frequency:** 1 day
- **Coverage:** Approximately 3 years per symbol
- **Purpose:**  
  - Long-horizon trend analysis  
  - Daily aggregation  
  - Rolling volatility and return computation  

### Quote Snapshots (Velocity Dataset)
- **Frequency:** Periodic polling (simulated streaming)
- **Storage Model:** Historical snapshots (append-only)
- **Purpose:**  
  - Demonstrate velocity-oriented ingestion  
  - Incremental dataset growth  
  - Near-real-time data persistence  

---

## Time & Market Assumptions
- **Market:** U.S. equities (NYSE and NASDAQ)
- **Timestamp Storage:**  
  All timestamps are normalized and stored in **UTC** to ensure consistency across ingestion, processing, and analytics stages.

---

## Big Data Dimensions (In Scope)

- **Volume:**  
  Persistent accumulation of multi-year historical time-series data across multiple symbols.

- **Velocity:**  
  Periodic ingestion of near-real-time quote snapshots, simulating continuous data arrival and incremental dataset growth.

---

## Planned Outputs (Persisted to MongoDB)

The following datasets are produced and stored persistently:

- **`raw_prices` collection:**  
  Immutable storage of all ingested historical prices and quote snapshots.

- **`daily_agg` collection:**  
  Daily aggregates including returns, log returns, moving averages, and rolling volatility.

- **`indicators` collection:**  
  Technical indicators such as RSI computed via aggregation pipelines.

- **Spark Outputs:**  
  - `spark_risk_return`: Per-symbol risk and return summaries  
  - `spark_monthly_returns`: Monthly aggregated returns  

All outputs are written back to MongoDB Atlas to ensure reproducibility.

---

## Scale Statement
The pipeline is designed to scale horizontally to:
- Additional symbols
- Longer historical time ranges
- Increased quote ingestion frequency  

without requiring architectural changes. Persistent NoSQL storage and distributed processing via Apache Spark remain the core scalability mechanisms.
