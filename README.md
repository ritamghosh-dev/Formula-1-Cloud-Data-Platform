# Formula 1 Cloud Data Platform

## Project Overview
This project is an end-to-end cloud-native data engineering solution designed to process, analyze, and visualize Formula 1 racing data. Leveraging **Microsoft Azure** and **Databricks**, the platform implements a robust **Medallion Architecture** (Bronze, Silver, Gold) to transform raw data into widespread analytical insights. The system handles large-scale data ingestion, incremental loading, and complex transformations to support effective decision-making.

## Solution Architecture
The platform is built on modern cloud capabilities, ensuring scalability, reliability, and performance.

### Data Flow
1.  **Ingestion (Bronze Layer)**: Raw data (JSON, CSV) is ingested from external sources into **Azure Data Lake Storage Gen2 (ADLS)**.
2.  **Transformation (Silver Layer)**: Data is cleaned, standardized, and stored in **Delta Lake** format. Schema enforcement and evolution are handled here.
3.  **Aggregation (Gold Layer)**: Business-level aggregates and KPIs are calculated for reporting.
4.  **Orchestration**: **Azure Data Factory (ADF)** pipelines manage the workflow dependencies and scheduling.
5.  **Visualization**: **Databricks SQL** Dashboards provide insights into driver performance, constructor standings, and race results.

![Solution Architecture](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/204160e2-ed22-4cbd-bb5d-5802e3c1933d)

---

## Tech Stack
-   **Cloud Provider**: Microsoft Azure
-   **Storage**: Azure Data Lake Storage Gen2 (ADLS)
-   **Compute & Processing**: Azure Databricks (Apache Spark 3.x)
-   **Orchestration**: Azure Data Factory (ADF)
-   **Data Format**: Delta Lake (Parquet-backed ACID transactions)
-   **Languages**: Python (PySpark), SQL
-   **Version Control**: Git & GitHub

## Key Features
-   **Medallion Architecture**: Structured Bronze (Raw), Silver (Augmented), and Gold (Business) layers.
-   **Incremental Loading**: Efficient processing of only new or modified data using Delta Lake's `merge` capabilities.
-   **Audit Columns**: Automatic tracking of `ingestion_date` and `source_system` for data lineage.
-   **Schema Enforcement**: Ensures data quality by validating incoming data structures.
-   **Partitioning**: Optimized storage layout (e.g., partitioning by `race_id`) for fast query performance.

---

## Detailed Pipelines

### 1. Ingestion Automation Pipeline
The ingestion layer handles various file formats and applies initial cleaning.
-   **Source**: [Ergast API](http://ergast.com/mrd/) (Open Source Formula 1 Data).
-   **Destination**: ADLS Gen2 (Bronze Container).
-   **Activities**:
    -   Reading CSV/JSON/API data.
    -   Renaming columns to snake_case.
    -   Adding audit metadata (Introduction of `ingestion_date`).

![Ingestion Pipeline 1](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/beb201ab-165e-4ac6-b981-628687f40a7c)
![Ingestion Pipeline 2](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/6aca9733-31ea-43fa-81f8-866f7427d9b6)

### 2. Transformation Automation Pipeline
The transformation layer bridges the gap between raw data and business insights.
-   **Activities**:
    -   Joining Driver, Constructor, and Race datasets.
    -   Handling Nulls and deduplication.
    -   Window functions for rolling aggregations.
    -   Writing to **Delta Tables** for ACID compliance.

![Transformation Pipeline 1](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/3f9390a9-78d5-4f37-a98d-928181a27bc8)
![Transformation Pipeline 2](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/77cb9ce5-5b13-41ab-ba58-9ace30fbc818)

---

## Analytics & Dashboards
Final analytical datasets are visualized to provide actionable insights.
-   **Driver Standings**: Analysis of points and wins over the season.
-   **Constructor Performance**: Aggregate metrics for team comparisons.

![Dashboard 1](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/821aa3bf-5d58-4371-a21b-db0b1f63b0ef)
![Dashboard 2](https://github.com/ritz-16/Formula-1-Cloud-Data-Platform/assets/79714302/d18e927f-217b-4994-a571-a792351242fb)

## Project Structure
```bash
Formula1_Cloud_Data_Platform/
├── ingestion/       # Notebooks for data ingestion (Bronze)
├── trans/           # Notebooks for transformations (Silver/Gold)
├── analysis/        # Ad-hoc analysis and visualizations
├── includes/        # Shared utility functions and configuration
└── utils/           # Helper scripts
```
