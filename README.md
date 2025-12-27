#🍽️ Food Establishment Inspections

## 📌 Project Overview

This project analyzes Food Establishment Inspection data from Chicago and Dallas to uncover insights that improve public health transparency.
The datasets come from open government portals and differ significantly in schema, requiring profiling, cleansing, standardization, dimensional modeling, and visualization.

The solution follows Medallion Architecture (Bronze → Silver → Gold) and produces business-ready BI dashboards using Power BI / Tableau.

⸻

## 🎯 Objectives

- Profile and assess data quality from multiple cities
- Standardize heterogeneous inspection schemas
- Design and implement a dimensional data model
- Apply SCD Type-2 to track historical changes
- Build analytical dashboards for inspection trends
- Demonstrate real-world data engineering best practices

⸻

## 🏙️ Data Sources

### Chicago Food Inspections

- Source: City of Chicago Open Data Portal
- URL: https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5

Dallas Food Establishment Inspections

- Source: Dallas Open Data Portal
- URL: https://www.dallasopendata.com/Services/Restaurant-and-Food-Establishment-Inspections-Octo/dri5-wcct

Reference Documents

- Dallas 47-Item Inspection Report
- Chicago Blank Inspection Report

⸻

## 🏗️ Architecture – Medallion Design

🟤 Bronze Layer (Raw)

- Raw ingestion from CSV / TSV files
- No transformations
- City-specific schemas retained
- Audit columns added (ingestion_date, source_file)

⚪ Silver Layer (Cleansed & Standardized)

- Data quality validations enforced
- Schema standardization across cities
- Violation parsing & normalization
- Bad records dropped using expectations

🟡 Gold Layer (Dimensional Model)

- Star schema implementation
- Business-ready fact & dimension tables
- SCD Type-2 implemented
- Optimized for BI tools

⸻

## Data Quality & Validation Rules (Silver Layer)

### Common Rules

- Restaurant Name cannot be NULL
- Inspection Date cannot be NULL
- Inspection Type cannot be NULL
- Zip Code must be valid & non-null
- Each inspection must have at least one unique violation

### Dallas-Specific Rules

- Violation score ≤ 100
- If score ≥ 90 → max 3 violations
- Inspection cannot be PASS if violations contain Urgent or Critical

### Chicago-Specific Rules

- Inspection Result cannot be NULL
- Violation codes/descriptions unstructured → parsed & standardized
- Violation score derived using:

  | Result             | Score |
  | ------------------ | ----- |
  | Pass               | 90    |
  | Pass w/ Conditions | 80    |
  | Fail               | 70    |
  | No Entry           | 0     |

## 🧩 Dimensional Data Model

### Fact Table

- fact_inspection
- inspection_id
- inspection_date_key
- restaurant_key
- location_key
- inspection_type_key
- result_key
- total_violation_score

### Dimension Tables

- dim_restaurant (SCD-2)
- dim_location
- dim_inspection_type
- dim_inspection_result
- dim_violation
- dim_date

### 📌 SCD Type-2 implemented on dim_restaurant

Tracks changes in restaurant name, ownership, or license over time.

⸻

## BI Dashboards (Power BI / Tableau)

Key Visuals

- Inspection results by city
  ![image](https://github.com/RakshithNarayanaswamy/Food-Quality-Inspection-analysis/blob/main/Images/image.png)
- Inspection outcomes by type
  ![image](https://github.com/RakshithNarayanaswamy/Food-Quality-Inspection-analysis/blob/main/Images/Screenshot%202025-12-27%20at%205.47.35%E2%80%AFPM.png)
- Risk category distribution
  ![image](https://github.com/RakshithNarayanaswamy/Food-Quality-Inspection-analysis/blob/main/Images/Screenshot%202025-12-27%20at%205.48.19%E2%80%AFPM.png)
- Most frequent violations
- Business-level inspection history
  ![image](https://github.com/RakshithNarayanaswamy/Food-Quality-Inspection-analysis/blob/main/Images/Screenshot%202025-12-27%20at%205.48.39%E2%80%AFPM.png)
- Location-based inspection trends
  ![image](https://github.com/RakshithNarayanaswamy/Food-Quality-Inspection-analysis/blob/main/Images/Screenshot%202025-12-27%20at%205.47.23%E2%80%AFPM.png)

Drill-Down Inspection Report

- Inspection #
- License #
- Violations (code & description)
- Inspector comments

⸻

## Tools & Technologies

- Databricks – Data processing & transformations
- Delta Lake – Storage & ACID compliance
- ER Studio / Navicat – Dimensional modeling
- Power BI / Tableau – Visualization
- Git & GitHub – Version control
- SQL / PySpark – Data engineering logic

---

## Business Value

- Improves public health transparency
- Enables risk-based inspection monitoring
- Supports policy and compliance analysis
- Demonstrates enterprise-grade data engineering
