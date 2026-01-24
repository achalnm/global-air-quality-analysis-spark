# Global Air Quality Analysis Using Spark

## Overview
This repository contains an end to end cloud data analytics pipeline implemented using Apache Spark (PySpark) to analyse global air quality data. The project integrates World Health Organization air pollution measurements with World Bank population statistics to move beyond raw environmental indicators and quantify population level exposure and health risk.

The pipeline performs large scale data ingestion, cleaning, standardisation, integration, and feature engineering to derive composite metrics such as a Pollution Index and a population weighted Health Risk Index. The final outputs enable cross country and temporal comparisons of air pollution intensity and its potential public health impact.

This project was developed as part of an MSc level Cloud Computing assignment and demonstrates scalable data engineering principles using distributed processing.

---

## Project Objectives
- Build a scalable Spark based ETL pipeline for heterogeneous environmental and demographic datasets  
- Clean and standardise inconsistent country identifiers across data sources  
- Handle high sparsity and missing sensor readings using statistically robust imputation strategies  
- Engineer meaningful composite metrics that combine pollution intensity with population exposure  
- Produce analysis ready datasets suitable for downstream visualisation and reporting  

---

## Datasets
This project uses two primary datasets. The cleaned datasets and the final analytical outputs are included in this repository. Links to the original raw datasets are provided within the project report.

### 1. WHO Ambient Air Quality Database
- Source: World Health Organization  
- Format: CSV (converted from the original Excel release)  
- Granularity: City level measurements per year  
- Key attributes:
  - Country name  
  - City or locality  
  - Measurement year  
  - PM2.5, PM10, NO2 concentrations in micrograms per cubic meter  

The dataset contains significant structural missingness, particularly for PM2.5 values in earlier years, which required advanced imputation techniques.

### 2. World Bank Population Dataset
- Source: World Bank Open Data  
- Format: CSV  
- Granularity: Country level population totals per year  
- Key attributes:
  - Country name  
  - ISO country code  
  - Year  
  - Population count  

This dataset is structurally complete but required string normalisation for accurate joins.

---

## Repository Contents
- `data/`
  - Cleaned and processed datasets
  - Exported analytical subsets in CSV and Parquet formats
- `notebooks/`
  - PySpark notebooks implementing the full ETL and analytics pipeline
- `report/`
  - Final project report describing methodology, architecture, and results
- `README.md`
  - Project documentation and usage details

The dataset files and the full written report are included in this repository. References and links to the original data sources are available in the report.

---

## Data Processing Pipeline
The pipeline is implemented entirely in PySpark and follows a modular, scalable design.

### 1. Data Ingestion
- Raw CSV files are loaded using SparkSession and spark.read.csv
- Schema inference is enabled to detect numeric and temporal fields
- Data is stored as distributed Spark DataFrames to support lazy evaluation

### 2. Standardisation and Entity Resolution
- Country name columns are normalised using lowercase conversion and whitespace trimming
- A broadcast dictionary resolves known country naming mismatches across datasets
- This step ensures complete and accurate joins between environmental and population data

### 3. Dataset Integration
- A Left Outer Join is performed using country name and year as the composite key
- WHO air quality data is preserved even when population values are missing

### 4. Missing Value Imputation
- High sparsity in pollutant measurements is addressed using Spark Window functions
- Missing PM2.5, PM10, and NO2 values are imputed using the median value for each country
- Countries with fully missing pollutant histories are left unfilled to avoid bias

### 5. Feature Engineering
Two composite metrics are derived:
- Pollution Index: Average of available pollutant concentrations
- Health Risk Index: Population weighted exposure metric calculated as  
  Health Risk Index = (Pollution Index × Population) / 1,000,000,000  

Double precision arithmetic is used to safely handle large population values.

### 6. Validation and Export
- Duplicate rows and invalid values are removed
- Final datasets are exported in both CSV and Parquet formats
- Additional analytical subsets are generated for high risk cases and recent year analysis

---

## Technologies Used
- Apache Spark (PySpark) for distributed data processing  
- Python for pipeline orchestration and analysis  
- Jupyter Notebooks as the development environment  
- CSV and Parquet formats for data storage and exchange  

---

## Key Contributions
- Demonstrates a full Spark based ETL pipeline aligned with cloud computing principles  
- Addresses real world data quality challenges such as sparsity and inconsistent identifiers  
- Introduces a population weighted Health Risk Index for realistic public health analysis  
- Produces reproducible and scalable analytics ready datasets  

---

## Contributors

| Name                     | Student ID  | Email ID                                   |
|--------------------------|-------------|--------------------------------------------|
| Achal Nanjundamurthy     | A00050840   | achalnm02@gmail.com         |
| Tejas Shiva Kumar        | A00050674   | tejas.shivakumar2@mail.dcu.ie              |

---
