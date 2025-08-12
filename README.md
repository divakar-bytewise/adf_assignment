# ADF Assignment: End-to-End Azure Data Factory & Delta Lake Pipeline

##  Overview

This repository demonstrates a full pipeline architecture using Azure Data Factory (ADF), ADLS Gen2, and Azure Databricks with Delta Lake structured across three layers:

- **Bronze layer**: Raw ingested data via ADF pipelines into ADLS containers
- **Silver layer**: Cleansed and transformed tables using Databricks (customer, product, store, customer_sales)
- **Gold layer**: Aggregated analytics output (`StoreProductSalesAnalysis`) combining store, product, and sales data

##  Repository Structure
##  Data Layers Explanation

### Bronze Layer
- ADF pipeline ingests raw files (`customer`, `product`, `store`, `sales`) into the `bronze/sales_view/` ADLS container.

### Silver Layer
- **Customer**: CamelCase headers converted to snake_case; `name` split; domain extracted from email; gender normalized; date-time split; `expenditure_status` added; upserted to Delta.
- **Product**: Snake_case headers; `sub_category` derived from `category_id`; upserted to Delta.
- **Store**: Snake_case headers; `store_category` derived from email domain; `created_at`, `updated_at` formatted; upserted.
- **Customer Sales**: Date columns parsed from ISO format; upserted to Delta.

### Gold Layer
- Builds a consolidated view (`StoreProductSalesAnalysis`) by joining `customer_sales`, `product`, and `store` data on `product_id` and `store_id`.
- Outputs are written as Delta under `gold/sales_view/StoreProductSalesAnalysis`.
