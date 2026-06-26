# 🏥 End-to-End Hospital Data Engineering Pipeline on Microsoft Azure
End-to-end Azure Data Engineering project implementing the Medallion Architecture (Bronze, Silver, Gold) using Azure Data Factory, Azure Data Lake Storage Gen2, Azure Databricks, Delta Lake, and SQL Server. Includes incremental loading, data validation, transformations, audit logging, SCD Type 1, and analytical reporting.

---

# Solution Workflow

### 1️⃣ Source System – SQL Server

The source system is an on-premises SQL Server database containing hospital operational data. The pipeline extracts data from five business tables:

* Accounts
* Departments
* Encounters
* Hospitals
* OrdersProcedures

---

### 2️⃣ Data Ingestion – Azure Data Factory

Azure Data Factory orchestrates the complete ingestion process using metadata-driven pipelines.

Key activities include:

* Lookup Watermark
* Dynamic UTC Folder Path Generation
* ForEach Table Processing
* Incremental Data Loading
* Watermark Update
* Audit Logging

This approach ensures that only newly added or modified records are processed during each pipeline execution.

---

### 3️⃣ Bronze Layer – Azure Data Lake Storage Gen2

Raw source data is stored in the Bronze layer without any transformations.

The pipeline automatically creates a dynamic UTC-based folder structure for every execution:

```text
bronze/
└── yyyy/MM/dd/HH/mm/
    ├── Accounts.csv
    ├── Departments.csv
    ├── Encounters.csv
    ├── Hospitals.csv
    └── OrdersProcedures.csv
```

This structure enables organized storage, easy traceability, and historical file management.

---

### 4️⃣ Secure Connectivity

Azure Databricks securely connects to ADLS Gen2 using a Service Principal.

Sensitive credentials such as Client ID, Client Secret, and Tenant ID are securely managed through Azure Key Vault, eliminating the need to hardcode secrets inside notebooks.

---

### 5️⃣ Data Validation

Before transformation, the raw datasets are validated to ensure data quality.

Validation checks include:

* Row Count Validation
* Null Value Validation
* Duplicate Primary Key Detection
* Data Quality Verification

Any issues identified during validation can be reviewed before data proceeds to the Silver layer.

---

### 6️⃣ Silver Layer Transformation

Data is cleaned, standardized, and transformed using PySpark before being stored as managed Delta tables.

Transformations performed include:

* Removing duplicate records
* Trimming unnecessary spaces
* Data type conversions
* Date standardization
* Metadata column creation
* Writing managed Delta tables

The Silver layer contains validated and analytics-ready datasets.

---

### 7️⃣ Gold Layer

The Gold layer stores business-ready curated datasets.

A Slowly Changing Dimension (SCD) Type 1 implementation updates existing records whenever attribute values change while maintaining the latest version of the data using Delta Lake MERGE operations.

---

### 8️⃣ Manual Sales Analytics

A separate Databricks notebook demonstrates analytical processing by creating a sample sales dataset.

Business metrics calculated include:

* Revenue
* Profit
* Highest Cost Product
* Top Customer by Quantity Purchased

The output is stored as a Delta table for reporting.

---

### 9️⃣ Power BI Reporting

The curated Gold layer is designed for business intelligence and reporting.

Power BI dashboards can be connected directly to Delta tables to visualize operational KPIs, hospital insights, encounter trends, and sales analytics.

---

### 🔟 Version Control

The complete project is maintained in GitHub, including:

* Azure Data Factory Pipelines
* Databricks Notebooks
* SQL Scripts
* Architecture Diagram
* Documentation

---

# 🛠️ Technologies Used

* Microsoft Azure
* Azure Data Factory
* Azure Data Lake Storage Gen2
* Azure Databricks
* PySpark
* Delta Lake
* Azure Key Vault
* SQL Server
* Power BI
* GitHub

---

# ✨ Key Features

* End-to-End Azure Data Engineering Pipeline
* Metadata-Driven Incremental Loading
* Dynamic UTC Folder Structure
* Watermark-Based Processing
* Audit Logging
* Medallion Architecture
* PySpark Data Validation & Transformation
* Managed Delta Tables
* Azure Key Vault Integration
* SCD Type 1 Implementation
* Power BI Ready Data Model
