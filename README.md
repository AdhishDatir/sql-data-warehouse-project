# SQL Data Warehouse Project

A modern data warehouse built with **SQL Server**, covering ETL pipelines, data modeling, and analytics. The project consolidates sales data from two source systems (CRM and ERP) into a clean, analytics-ready star schema using the **Medallion Architecture** (Bronze → Silver → Gold).

---

## Project Overview

This project covers the full data engineering workflow:

1. **Data Architecture**: A three-layer warehouse (Bronze, Silver, Gold).
2. **ETL Pipelines**: Extract, transform, and load data from CSV source files into SQL Server.
3. **Data Modeling**: Fact and dimension tables in a star schema, built for reporting.
4. **Analytics**: SQL queries for customer behavior, product performance, and sales trends.

### Skills Demonstrated
SQL Development · Data Engineering · ETL Pipeline Development · Data Modeling · Data Quality & Cleansing · Data Analytics

---

## Architecture

The warehouse follows the Medallion Architecture:

| Layer | Purpose | Object Type | Load Method |
|-------|---------|-------------|-------------|
| **Bronze** | Raw data, ingested as-is from the sources | Tables | Batch, full load (truncate & insert) |
| **Silver** | Cleaned, standardized, and normalized data | Tables | Batch, full load (truncate & insert) |
| **Gold** | Business-ready data in a star schema | Views | No load, views over Silver |

**Data flow:** `CSV files (CRM, ERP)` → **Bronze** → **Silver** → **Gold** → `Reports / Analytics`

> Add your architecture diagram here: `docs/data_architecture.png`

---

## Repository Structure

```
sql-data-warehouse-project/
│
├── datasets/     # Raw source data (CRM and ERP CSV files)
├── docs/         # Architecture diagrams, data models, naming conventions, data catalog
├── scripts/      # SQL scripts for each layer
│   ├── bronze/         # DDL and load procedures for raw data
│   ├── silver/         # Cleansing and transformation scripts
│   └── gold/           # Views for the dimension and fact tables
├── tests/        # Data quality checks and validation queries
└── README.md
```

---

## Data Sources

Two source systems, delivered as CSV files:

- **CRM**: customer info, product info, sales details
- **ERP**: customer demographics, customer locations, product categories

---

## Data Model (Gold Layer)

The Gold layer exposes a star schema:

- `dim_customers`: customer attributes merged from CRM and ERP
- `dim_products`: product attributes with category information
- `fact_sales`: sales transactions linked to the dimension tables

---

## ETL Highlights

**Bronze**
- Loads raw CSV data with `BULK INSERT`
- No transformations, to preserve source fidelity

**Silver**
- Removes duplicates and trims unwanted spaces
- Handles nulls and invalid values
- Standardizes categorical values (e.g. gender, marital status)
- Fixes invalid dates and recalculates inconsistent sales and price values
- Derives new columns where needed

**Gold**
- Joins CRM and ERP data
- Integrates entities and generates surrogate keys
- Exposes the final dimension and fact views

---

## Getting Started

### Prerequisites
- [SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (or any SQL Server edition)
- [SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) or Azure Data Studio
- [Git](https://git-scm.com/)

### Setup

1. **Clone the repository**
```bash
   git clone https://github.com/AdhishDatir/sql-data-warehouse-project.git
   cd sql-data-warehouse-project
```

2. **Initialize the database and schemas**
   Run the initialization script in `scripts/` to create the `DataWarehouse` database and the `bronze`, `silver`, and `gold` schemas.

3. **Update file paths**
   In the Bronze load script, point the `BULK INSERT` paths to where the CSV files from `datasets/` live on your machine.

4. **Run the pipeline in order**
```sql
   -- Load raw data
   EXEC bronze.load_bronze;

   -- Clean and transform
   EXEC silver.load_silver;
```
   Then run the Gold scripts to create the views.

5. **Validate**
   Run the queries in `tests/` to check data quality and integrity.

---

## Testing

The `tests/` folder holds quality checks, for example:
- Duplicate or null primary keys
- Unwanted whitespace in string fields
- Invalid date ranges and orders
- Consistency between sales, quantity, and price
- Referential integrity between the fact and dimension views

---

## Documentation

See the `docs/` folder for:
- Architecture and data flow diagrams
- Data model / star schema diagram
- Naming conventions
- Gold layer data catalog

---

## Naming Conventions

- **Style:** `snake_case`, English only
- **Bronze/Silver tables:** `<source>_<entity>` (e.g. `crm_cust_info`)
- **Gold objects:** `<category>_<entity>` (e.g. `dim_customers`, `fact_sales`)
- **Surrogate keys:** `<table>_key`
- **Stored procedures:** `load_<layer>`

---

## Author

**Adhish Datir**
[GitHub](https://github.com/AdhishDatir)

---

