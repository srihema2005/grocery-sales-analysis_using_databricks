# Grocery Sales Data Engineering Project

## 📌 Project Overview

This project implements an end-to-end **Grocery Sales Data Engineering pipeline on Microsoft Azure**.  
The solution ingests raw grocery datasets, performs data quality validation and cleansing, applies business transformations, and prepares analytics-ready **Silver and Gold layer tables** using a **Star Schema**.

The pipeline follows the **Medallion Architecture**:

**Raw Source → Bronze → Silver → Silver Business Transformation → Silver Enriched → Gold**

---

## 🏗️ Architecture

### 1. Source Layer
The project receives raw TXT/CSV-style datasets such as:

- `train.txt` – Sales history
- `test.txt` – Future sales/test data
- `stores.txt` – Store master data
- `transactions.txt` – Store transactions
- `oil.txt` – Daily oil price
- `holidays_events.txt` – Holidays and events

Raw files are stored in **Azure Data Lake Storage Gen2 (ADLS Gen2)**.

### 2. Ingestion & Control Layer

Metadata and control information is maintained for pipeline execution and monitoring.

Main control areas include:

- Source file metadata
- File ingestion control
- Pipeline run control
- Batch control

### 3. Bronze Layer – Raw Ingestion

The source data is loaded into raw Bronze tables without major business transformations.

Example tables:

- `raw_train`
- `raw_test`
- `raw_stores`
- `raw_transactions`
- `raw_oil`
- `raw_holidays_events`

Common metadata columns include:

- `ingestion_timestamp`
- `source_file_name`
- `source_path`
- `batch_id`
- `pipeline_run_id`
- `record_hash`

### 4. Schema Validation & Data Quality

Data quality checks are performed before moving valid records forward.

Validation includes:

- Schema validation
- Data type validation
- Null checks
- Duplicate checks
- Range/domain checks
- Referential integrity checks

Invalid records are moved to quarantine/rejected areas for further investigation.

### 5. Silver Layer – Clean & Curated

The Bronze data is cleaned and standardized.

Typical operations include:

- Null handling
- Data type conversion
- Invalid value correction
- Duplicate removal
- Standard date formatting
- Trimming and case standardization
- Business rule enforcement
- Referential validation

Example tables:

- `clean_train`
- `clean_test`
- `clean_stores`
- `clean_transactions`
- `clean_oil`

### 6. Silver Business Transformation Layer

Business transformations are applied using:

- Joins
- Aggregations
- Derived columns
- Calculated metrics
- Business rules
- Standardization
- Performance metrics

### 7. Silver Enriched Intermediate Layer

Data is enriched with reference information before creating dimensions and facts.

Examples include:

- Store attributes
- Product attributes
- Holiday attributes
- Oil price information
- Calculated business metrics

---

# ⭐ Gold Layer – Star Schema

The final analytics model follows a **Star Schema** consisting of dimension tables and a central fact table.

## Dimension Tables

### `dim_date`

Contains date-related attributes:

- `date_key` – Primary Key
- `date`
- `year`
- `month`
- `week_of_year`
- `day_of_week`

### `dim_store`

Contains store-related attributes:

- `store_nbr` – Primary Key
- `city`
- `state`
- `store_type`
- `region`
- `cluster`

### `dim_product`

Contains product attributes:

- `product_key` – Primary Key
- `product_family`
- `product_category`
- `product_subcategory`
- `brand`
- `product_name`
- `unit_size`

## Fact Table

### `fact_sales`

Contains sales measures and foreign keys to the dimensions.

Key columns:

- `sales_id` – Primary Key
- `date_key` – Foreign Key
- `store_nbr` – Foreign Key
- `product_key` – Foreign Key
- `sales_amount`
- `sales_quantity`
- `unit_price`
- `discount_amount`
- `total_amount`

### Fact Table Grain

**One row represents the sales of one product in one store on a given date.**

---

## 🔄 Data Flow

```text
Source TXT/CSV Files
        |
        v
Azure Data Lake Storage Gen2
        |
        v
Ingestion & Control
        |
        v
Bronze Layer
        |
        v
Schema Validation & Data Quality
        |
        +---- Invalid Records --> Quarantine
        |
        v
Silver Clean & Curated
        |
        v
Business Transformations
        |
        v
Silver Enriched
        |
        v
Dimension Creation
        |
        +----> dim_date
        +----> dim_store
        +----> dim_product
        |
        v
fact_sales
        |
        v
Gold Layer
        |
        v
Analytics / Reporting
```

---

## 🛠️ Technologies Used

- **Azure Data Lake Storage Gen2** – Data storage
- **Azure Data Factory** – Data ingestion and orchestration
- **Azure Databricks** – Data processing and transformations
- **Apache Spark / PySpark** – Distributed data processing
- **Delta Lake** – Reliable table storage and ACID transactions
- **dbt** – SQL-based transformation and data modeling
- **Apache Airflow** – Workflow orchestration/scheduling
- **Slack** – Pipeline alerts and notifications
- **Git/GitHub** – Source code and version control

---

## 📂 Suggested Repository Structure

```text
grocery-sales-data-engineering/
│
├── README.md
│
├── data/
│   └── README.md
│
├── notebooks/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── dbt/
│   ├── models/
│   │   ├── bronze/
│   │   ├── silver/
│   │   └── gold/
│   ├── sources.yml
│   └── dbt_project.yml
│
├── pipelines/
│   └── azure-data-factory/
│
├── sql/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── config/
│   ├── data_quality_rules/
│   └── pipeline_config/
│
├── docs/
│   └── architecture/
│
└── tests/
    └── data_quality/
```

---

## 🔍 Data Quality & Error Handling

The pipeline maintains validation and audit information to improve reliability and traceability.

Important checks include:

- File arrival validation
- Record count validation
- Schema validation
- Null validation
- Duplicate validation
- Data type validation
- Referential integrity
- Business rule validation

Rejected records are stored separately in quarantine tables/locations.

---

## 📊 Business Metrics

The Gold layer can support analytics such as:

- Total Sales
- Total Transactions
- Average Sales per Store
- Sales by Product Family
- Sales by Product Category
- Sales by Region
- Sales by Store
- Sales by Date
- Product Performance
- Inventory-related metrics

---

## 🔐 Governance & Monitoring

The project includes audit, logging, and monitoring concepts such as:

- Pipeline execution logs
- Transformation audit logs
- Data quality audit logs
- Error logs
- Batch tracking
- Pipeline run tracking
- Failure alerts
- Data quality alerts

Slack/email notifications can be used to notify the team about pipeline failures, data quality issues, or other important events.

---

## 👩‍💻 My Role & Responsibilities

My primary responsibility in this project was working on the **Gold Layer and data modeling**.

Key responsibilities:

1. Designed and prepared Gold layer tables.
2. Created the Star Schema with dimension and fact tables.
3. Developed SQL/dbt transformations for business-ready data.
4. Implemented joins, aggregations, derived columns, and business metrics.
5. Performed data validation and quality checks.
6. Worked with Databricks and Delta tables.
7. Supported pipeline orchestration and monitoring using Airflow.
8. Worked with Slack alerts for pipeline notifications.
9. Maintained project code and SQL/dbt files in GitHub.

---

## 🚀 Project Outcome

The project converts raw grocery sales data into a structured, validated, enriched, and analytics-ready data model.

The final **Gold Star Schema** enables efficient reporting and analysis of:

**Sales + Products + Stores + Dates**

while maintaining data quality, traceability, and scalable Azure-based data processing.

---

## 📌 Key Design Principle

The project follows:

**Bronze = Raw Data**  
**Silver = Cleaned & Transformed Data**  
**Gold = Business-Ready Analytics Data**

The Gold layer is optimized for consumption by reporting and analytics tools such as **Power BI**.
