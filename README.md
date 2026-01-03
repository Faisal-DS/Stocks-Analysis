# Stock Market Analysis Using Big Data Storage and Processing

This is the repository for Big Data Project - Stock Market Analysis Using Big Data Storage and Processing System

## Project Overview

This project was developed as a semester project for COMP-548DL – Big Data Management and Processing, under the supervision of Dr. Demetris Trihinas.

The objective of the project is to design and implement an end-to-end Big Data analytics pipeline for financial time-series data, demonstrating persistent storage, scalable data ingestion, and distributed processing using modern Big Data technologies.

The system focuses on U.S. equity market data and illustrates how raw market data can be transformed into analytical insights without relying on in-memory or spreadsheet-style workflows.

## Big Data Challenges Addressed

The project explicitly addresses two key Big Data dimensions:

Volume: Multi-year historical price data accumulated across multiple equity instruments.

Velocity: Periodic ingestion of near-real-time quote snapshots simulating continuous data arrival.

Traditional in-memory tools (e.g. pandas-based pipelines) are intentionally avoided for ingestion and analytics due to scalability limitations.
The veracity dimension is intentionally excluded from scope in accordance with course guidance.

# Dataset and Assumptions
## Asset Universe

The analysis focuses on the following eight U.S. equity instruments:

AAPL, MSFT, NVDA, AMZN, TSLA, APP, SMCI, SPY

This selection includes large-cap stocks, high-growth equities, and a broad-market ETF (SPY) to enable comparative analysis across different market behaviors.

## Time and Market Assumptions

Market: U.S. equities (NYSE / NASDAQ)

Timezone: All timestamps normalized to UTC

Historical coverage: ~3 years of daily data

Intraday data: Periodic quote snapshots

# System Architecture

The project follows a layered Big Data architecture:

Data Ingestion → MongoDB Atlas → Apache Spark → Visualization

Technologies Used

MongoDB Atlas – Persistent NoSQL storage

Apache Spark (PySpark) – Distributed processing

Python – Ingestion and orchestration

Jupyter Notebooks – Execution and visualization

# Data Model and Storage Design

All data is stored persistently in MongoDB Atlas using a structured and scalable data model:

raw_prices – Immutable raw market data (daily bars and quote snapshots)

daily_agg – Daily aggregates and rolling statistics

indicators – Technical indicators (e.g. RSI-14)

spark_risk_return – Spark-derived risk/return metrics

spark_monthly_returns – Spark-derived monthly returns

Compound unique indexes on (symbol, timestamp, interval) enforce data integrity, prevent duplicates, and support idempotent ingestion.

# Data Ingestion and Processing
Batch Ingestion (Volume)

Historical daily OHLCV data ingested for all symbols

Stored persistently with interval = "1day"

Velocity-Oriented Ingestion

Periodic polling of quote-level data

Each snapshot stored with interval = "quote"

Data Validation and Cleaning

Duplicate prevention via unique indexes

Validation of date ranges and record counts

Financial sanity checks (positive prices, valid volumes)

Distributed Processing with Apache Spark

Apache Spark is used to perform distributed analytics on persistently stored data, including:

Distributed joins across collections

Per-symbol risk and return analysis

Monthly return aggregation using window functions

All Spark outputs are written back to MongoDB, ensuring reproducibility and reuse.

# Analytics and Insights

The project produces several analytical insights, including:

Price trends using moving averages (SMA-20, SMA-50)

Risk analysis using rolling volatility

Momentum analysis using RSI-14

Cross-asset risk–return comparisons

Monthly performance comparisons

These insights are visualized in the final stage of the project.

# Proposal Alignment and Deviations

The final implementation is strongly aligned with the initial project proposal. Minor, justified deviations include:

Introduction of Stooq as a historical data source due to Alpha Vantage free-tier limitations.

Execution of Spark in local mode due to environment constraints.

Additional analytics (risk/return and monthly returns) beyond the original scope.

All deviations were documented and motivated by practical considerations and do not affect the project’s core objectives.

# Reproducibility Instructions
## Requirements

Python 3.10+

MongoDB Atlas account

Java 17 (for Spark)

Required Python libraries (see notebooks)

## Steps to Reproduce

Clone this repository

Create a .env file with:

MongoDB connection string

Install required dependencies

Execute notebooks in the following order:

Step 3 – Data Ingestion

Step 6 – Distributed Processing

Step 7 – Visualization

No code modifications are required to reproduce results.

# Project Video

A short demonstration video (≤ 5 minutes) illustrating the system architecture, analytics, and insights is available on YouTube:

[YouTube Link – to be added]

# Final Notes

This project demonstrates how Big Data technologies can be applied to financial analytics in a scalable, reproducible, and industry-aligned manner, fully satisfying the objectives of the COMP-548DL course.
