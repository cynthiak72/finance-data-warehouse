# Finance Data Warehouse & Executive Analytics Platform

##  Project Overview

This project demonstrates the design and implementation of an end-to-end
financial data warehouse and executive analytics platform.

The project applies Kimball dimensional modelling principles to transform
financial and operational data into a structured analytical data warehouse
and Power BI reporting solution.

The objective is to demonstrate how raw transactional data can be transformed
into reliable, historical and analysis-ready information for financial
decision-making.

---

## Project Objectives

The project aims to:

- Design a dimensional data warehouse using Kimball principles
- Identify business processes and define fact table grain
- Design fact and dimension tables using a star schema
- Implement Slowly Changing Dimensions (SCD)
- Build an ETL/ELT data pipeline
- Implement data quality and validation checks
- Develop a Power BI semantic model
- Create executive financial performance reporting
- Compare Kimball, Inmon and Data Vault architectures
- Document architectural decisions and trade-offs

---

## Solution Architecture

The solution will follow an end-to-end analytical architecture:

```text
Operational / Source Data
          │
          ▼
      Staging Layer
          │
          ▼
  Dimensional Data Warehouse
          │
          ├── Dimension Tables
          │
          └── Fact Tables
          │
          ▼
   Power BI Semantic Model
          │
          ▼
 Executive Finance Analytics
