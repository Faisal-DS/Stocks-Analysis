# Project Scope & Dataset Plan

## Asset Universe
Symbols (8): AAPL, MSFT, NVDA, AMZN, TSLA, APP, SMCI, SPY

## Data Sources
Primary provider: Alpha Vantage API (JSON time-series).
Fallback provider (only if needed): Yahoo Finance.

## Datasets
1) Daily Historical Prices
- Frequency: 1 day
- Coverage: 3 years
- Purpose: long-horizon trend analysis, daily aggregation, long-term volatility

2) Intraday Prices
- Frequency: 5-minute bars
- Ingestion rate: every 5 minutes
- Purpose: velocity demonstration, intraday patterns, short-term volatility, near-real-time updates

## Time & Market Assumptions
- Market: US equities
- Timestamp storage: UTC (all records normalized to UTC)

## Big Data Dimensions (In Scope)
- Volume: persistent accumulation of multi-symbol time-series over years
- Velocity: repeated intraday ingestion every 5 minutes
- Veracity: explicitly out of scope

## Planned Outputs (Produced via Spark and stored back to MongoDB)
- daily_agg collection: per-symbol daily OHLC and volume summaries
- indicators collection: moving averages (MA 5/20/50), rolling volatility, intraday pattern aggregates

## Scale Statement
The pipeline is designed to scale to hundreds of symbols and longer intraday histories without architectural changes
(persistent storage + distributed processing remain the same).
