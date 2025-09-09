# CDR Processing & BI System

## 📘 Overview
This project implements a **telecom-grade ETL pipeline** that ingests, normalizes, and consolidates **Call Detail Records (CDRs)** from multiple network elements (AWSd, AXE, ZTE, Huawei, Softswitch).  

The system supports **multi-format CDRs (binary & plain text)**, consolidates data in a **MySQL cluster**, and provides **near real-time analytics** via a BI module and reporting engine.

---

## 📐 Architecture

- **Input Sources**: Telecom switches producing binary/plain text CDRs.  
- **Ingestion & Repository**: Main Repo stores raw files.  
- **ETL Layer**: Orchestrated by CRONTAB & Bash, with Java parsers for normalization.  
- **Database Cluster**: MySQL with stored procedures and functions for business logic.  
- **BI Module**: Web-based services (PHP/JavaScript) for real-time queries and reporting.  
- **Reporting Engine**: Bash scripts producing consolidated reports and CSV exports.  
- **Data Warehouse**: Centralized MySQL-based repository for historical analysis.  

Architecture diagrams can be found in the [`diagrams`](./diagrams) folder.

---

## 📂 Project Structure

## 📂 Project Structure

```text
├── cdr-processing-system/    # Project 1: Telecom CDR ETL + BI
│   │── README.md             # Project-specific documentation
│   │── diagrams/             # PlantUML diagrams (C4 model, UML)
│   │── docs/                 # High-level docs (PDF/Word/Markdown)
│   │── src/                  # Java, PHP, Bash source code
│   │── scripts/              # Bash automation and reporting scripts

```

---

## 🔧 Tech Stack

- **Languages**: Java, PHP, JavaScript, Bash  
- **Database**: MySQL (Cluster + Data Warehouse)  
- **Scheduling**: CRONTAB  
- **Architecture Modeling**: PlantUML (C4 diagrams)  

---

## 🚀 Key Features

- Multi-format CDR ingestion (binary/plain text)  
- ETL process with orchestration and transformation  
- Data consolidation in MySQL cluster  
- Web-based BI services with ~2h data freshness  
- Automated reporting engine (CSV, consolidated reports)  

---
