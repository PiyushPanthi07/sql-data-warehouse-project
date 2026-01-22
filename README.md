# sql-data-warehouse-project
Building a modern data warehouse with SQL Server, including ETL processes, data modelling, and analytics.

Data Warehouse Architecture: Sales Data Consolidation📖 Project OverviewThis project involves the # Data Warehouse Architecture: Sales Data Consolidation

![Status](https://img.shields.io/badge/Status-Design_Complete-success)
![Platform](https://img.shields.io/badge/Platform-SQL_Server-blue)
![Architecture](https://img.shields.io/badge/Architecture-Medallion-orange)
![Domain](https://img.shields.io/badge/Domain-Data_Engineering-8A2BE2)

## 📖 Project Overview

This project involves the end-to-end design and implementation of a modern Data Warehouse (DWH) using **Microsoft SQL Server**. The primary objective is to consolidate sales data from disparate source systems (ERP and CRM) into a single, user-friendly data model that enables analytical reporting and informed decision-making.

The system is designed to handle data quality issues, integrate multiple sources, and provide a scalable foundation for Business Intelligence (BI) without the need for complex historization.

## 🎯 Business Requirements

The data warehouse addresses the following key business needs:
* **Consolidation:** Import and merge data from two distinct source systems: **ERP** and **CRM** (provided as CSV exports).
* **Data Quality:** Rigorous cleaning and standardization to fix "messy" raw data before analysis.
* **Integration:** Create a unified data model designed specifically for Analytics and Reporting.
* **Currency:** Focus on the latest dataset snapshots (no historization required).
* **Documentation:** Deliver clear documentation of the data model to support business users and analysts.

## 🏗️ Data Architecture

This project utilizes the **Medallion Architecture** (Multi-Hop Architecture), organizing data into three distinct layers based on quality and purpose. This approach ensures a strict **Separation of Concerns**, where each layer handles specific tasks (ingestion, cleaning, business logic) without overlapping responsibilities.

### Architecture Diagram
`Sources (CSV) -> Bronze Layer (Raw) -> Silver Layer (Clean) -> Gold Layer (Business)`

### Layer Specifications

| Layer | Name | Role | Object Type | Load Strategy | Transformations |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Layer 1** | **Bronze** | Raw Data Landing | Tables | Full Load (Truncate/Insert) | **None.** Data is ingested "as-is" for traceability and debugging. |
| **Layer 2** | **Silver** | Clean/Standardized | Tables | Full Load (Truncate/Insert) | **Technical Transformations.** Data cleansing, normalization, and standardization. No business logic. |
| **Layer 3** | **Gold** | Business Ready | Views | Virtual (No Load) | **Business Transformations.** Aggregations, calculations, and data modeling (Star Schema) for reporting. |

## 🛠️ Technical Implementation Details

### 1. The Bronze Layer (Raw)
* **Purpose:** Acts as the landing zone for raw CSV data.
* **Rule:** The schema must mirror the source files exactly. If data is bad in the source, it stays bad in Bronze.
* **Access:** Restricted to Data Engineers only.

### 2. The Silver Layer (Cleansing)
* **Purpose:** To produce a clean, consistent version of the data.
* **Activities:**
    * Removing duplicates.
    * Standardizing formats (dates, strings).
    * Handling null values.
    * Enriching data (technical derivations).
* **Access:** Data Engineers, Data Analysts (for ad-hoc exploration).

### 3. The Gold Layer (Curated)
* **Purpose:** The consumption layer for BI tools (Power BI, Tableau) and business users.
* **Design:** Implemented as **SQL Views** to ensure dynamic access and agility.
* **Activities:**
    * Joining tables from Silver.
    * Applying business rules and KPIs.
    * Modeling data into Fact and Dimension structures.

## 💾 Data Sources

* **ERP System:** Source data provided in CSV format (Folders/Files interface).
* **CRM System:** Source data provided in CSV format (Folders/Files interface).

## 🧰 Tech Stack

* **Database:** Microsoft SQL Server
* **ETL/Orchestration:** T-SQL Stored Procedures
* **Design Tools:** Draw.io (for Architecture Mapping)

## 🚀 Getting Started

1.  **Clone the Repository**
2.  **Setup Database:** Execute the `init_database.sql` script to create the database and schemas (`bronze`, `silver`, `gold`).
3.  **Ingest Data:** Run the Bronze layer ingestion scripts to load CSVs into the Bronze tables.
4.  **Process Pipeline:** Execute the stored procedures to promote data from Bronze → Silver → Gold.
5.  **Connect BI:** Point your BI tool (e.g., Power BI) to the views in the `gold` schema.

## 🤝 Contributing

Contributions regarding data quality rules or additional business metrics for the Gold layer are welcome. Please ensure any PR follows the established **Separation of Concerns** principle (e.g., no cleaning logic in the Gold layer).

---
*Project developed as part of the Data Engineering Portfolio Series.*
