# Azure-Databricks-Medallion-ETL-Spark-Project
An end-to-end cloud data engineering pipeline built on **Azure Databricks** utilizing the **Medallion Architecture (Bronze -> Silver -> Gold)**, **PySpark**, **Delta Live Tables**, and **Spark Streaming**. This project demonstrates industry best practices for high-scale data ingestion, dimensional modeling (Star Schema), and handling **Slowly Changing Dimensions (SCD)**.

## 🏗️ Architecture Overview
### End-to-end pipeline 
<img width="1788" height="608" alt="Screenshot 2026-08-13 at 9 53 31 PM" src="https://github.com/user-attachments/assets/30ba6044-da73-403c-b6d1-2132d75863d3" />

The pipeline processes raw transactional data into highly structured, analytics-ready business intelligence assets through three distinct layers:

1. **Bronze Layer (Ingestion & Parameters)**
   * **Purpose:** Raw data landing zone.
   * **Components:** Ingests raw source data streams and tracks operational metadata using parameter notebooks. It preserves data in its original format with append-only logs.
2. **Silver Layer (Cleaning & Enrichment)**
   * **Purpose:** Cleansed, conformed, and standardized tables.
   * **Components:** Processes data via `Silver_Customers.ipynb`, `Silver_Products.ipynb`, and `Silver_Orders.ipynb`. Enforces data quality, structural schemas, deduplication, and manages history using **SCD Type 1 / Type 2** strategies.
3. **Gold Layer (Analytical Modeling)**
   * **Purpose:** Business-level aggregates and star schema structures optimized for BI tools (e.g., Power BI) and advanced analytics.
   * **Components:** Curated via `Gold_Customers.ipynb`, `Gold_Products.ipynb`, and `Gold_orders.ipynb` into dedicated Dimension and Fact tables.

---

## 🛠️ Technology Stack

* **Cloud Platform:** Microsoft Azure
* **Compute / Processing Engine:** Azure Databricks, Apache Spark (PySpark)
* **Storage & Frameworks:** Delta Lake, Delta Live Tables (DLT)
* **Processing Paradigms:** Spark Streaming (Real-time/Micro-batch ingestion) & Batch Processing

---

## 📂 Repository Structure

```text
├── Databricks-NoteBook.ipynb   # Main orchestration notebook / configuration
├── Parameters_bronze.ipynb     # Parameterization and Bronze layer ingestion setup
│
├── Silver_Customers.ipynb      # Silver layer: Customer data cleaning and SCD logic
├── Silver_Products.ipynb       # Silver layer: Product validation and schema mapping
├── Silver_Orders.ipynb         # Silver layer: Order transactions processing
│
├── Gold_Customers.ipynb        # Gold layer: Customer Dimension (Dim_Customers)
├── Gold_Products.ipynb         # Gold layer: Product Dimension (Dim_Products)
└── Gold_orders.ipynb           # Gold layer: Orders Fact Table (Fact_Orders)
```

---

## 🚀 Key Engineering Implementations

### 1. Delta Live Tables (DLT) & Streaming
Leverages Spark Streaming to automatically detect new files in storage containers, enabling low-latency incremental data processing. DLT pipelines enforce data expectations (data quality rules) seamlessly.

### 2. Slowly Changing Dimensions (SCD)
* **SCD Type 1:** Applied to fields where history tracking is unnecessary (overwriting old data with new data, such as minor spelling corrections).
* **SCD Type 2:** Applied to critical historical fields (e.g., Customer Address changes). Uses tracking columns like `is_current`, `start_date`, and `end_date` to retain historical records for accurate retrospective reporting.

### 3. Star Schema Dimensional Modeling
Transforms normalized transactional data into an optimized analytical model consisting of:
* `Dim_Customers`
* `Dim_Products`
* `Fact_Orders`

---

## 🔧 Setup and Deployment Instructions

### Prerequisites
* Active **Azure Subscription**
* **Azure Databricks Workspace** configured with a Unity Catalog or Hive Metastore
* **Azure Data Lake Storage (ADLS Gen2)** for landing raw data files

### Deployment Steps
1. **Clone the Repository:** Clone these notebooks into your local machine or connect your GitHub repository directly to your Azure Databricks Workspace using **Databricks Repos**.
2. **Configure Storage:** Update the base data paths or environment parameters in `Parameters_bronze.ipynb` to point to your cloud storage containers.
3. **Orchestrate Pipelines:** 
   * For **Batch/Interactive execution**, run `Databricks-NoteBook.ipynb` to trigger notebooks sequentially.
   * For **Production scaling**, create a **Databricks Workflow Job** or a **Delta Live Tables Pipeline** and attach these notebooks as pipeline tasks.


