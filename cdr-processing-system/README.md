# CDR Processing & BI System

## 📘 Overview
This project implements a **telecom-grade ETL pipeline** that ingests, normalizes, and consolidates **Call Detail Records (CDRs)** from multiple network elements (AWSd, AXE, ZTE, Huawei, Softswitch).  

The system supports **multi-format CDRs (binary & plain text)**, consolidates data in a **MySQL cluster**, and provides **near real-time analytics** via a BI module and reporting engine.

---

## 📐 Architecture Diagrams (C4 Model)

The system architecture is documented using the **C4 Model** for software architecture, supported with PlantUML diagrams.

### 1. System Context (Level 1)
Shows the **CDR Processing System** as a black box, its external dependencies, and consumers.

![System Context](./diagrams/C4_L1.png)

📄 [PlantUML source](./docs/C4_L1.puml)

**Explanation:**
- **Telecom Switches / Softswitches** send raw CDRs (binary, plain text).  
- **CDR Processing System** ingests, processes, and makes data available.  
- **External BSS System** queries processed data for billing and business operations.  

---

### 2. Container Diagram (Level 2)
Breaks down the **CDR Processing System** into major containers and their interactions.

![Container Diagram](./diagrams/C4_L2.png)

📄 [PlantUML source](./docs/C4_L2.puml)

**Key Containers:**
- **Main Repository** → Raw file storage.  
- **ETL Orchestration** → Crontab + Bash + Java parsing.  
- **MySQL Cluster** → Centralized storage with procedures/functions for business rules.  
- **BI Module** → Web services and dashboards for near real-time analytics.  
- **Reporting Engine** → Bash-based scripts to generate consolidated CSV reports.  

---

### 3. Component Diagram (Level 3 - ETL)
Details the **ETL Container**, showing its internal components.

![ETL Component Diagram](./diagrams/C4_L3_ETL.png)

📄 [PlantUML source](./docs/C4_L3_ETL.puml)

**Components:**
- **Crontab Scheduler** → Triggers ETL jobs periodically.  
- **Bash Orchestration Scripts** → Coordinate and call Java processes.  
- **Java Parsers** → Parse, normalize, and transform CDRs.  
- **Data Loader** → Loads structured data into the MySQL Cluster.  

**External Context:**
- **Main Repository** → Provides raw CDR files.  
- **MySQL Cluster** → Stores processed data.  

---

#### 3.2 Reporting Engine
Shows the internal structure of the **Reporting Engine**.

![Reporting Component Diagram](./diagrams/C4_L3_REPORT.png)  
📄 [PlantUML source](./docs/C4_L3_REPORT.puml)

**Components:**
- **Job Scheduler** → Cron-based task scheduler.  
- **Report Generator** → Queries MySQL and builds reports.  
- **CSV Exporter** → Formats and exports consolidated data into CSV.  

**External Context:**
- **MySQL Cluster** → Provides processed data for reports.  
- **External BSS System** → Consumes CSV exports and reports.  

---

#### 3.3 BI Module
Shows the internal structure of the **BI Module**.

![BI Component Diagram](./diagrams/C4_L3_BI.png)  
📄 [PlantUML source](./docs/C4_L3_BI.puml)

**Components:**
- **BI Web UI** → Dashboards, analytics, and visualizations.  
- **BI Web Services (APIs)** → Expose processed data for integration.  
- **Authentication & Access Control** → Ensures secure access to data.  

**External Context:**
- **MySQL Cluster** → Data source for analytics and dashboards.  
- **External BSS System** → Consumes BI services for billing and operations.  

---


## 🔧 Tech Stack

- **Languages**: Java, PHP, JavaScript, Bash  
- **Database**: MySQL (Cluster + Data Warehouse)  
- **Scheduling**: CRONTAB  
- **Architecture Modeling**: PlantUML (C4 model)  

---

## 🚀 Key Features

- Multi-format CDR ingestion (binary/plain text)  
- ETL process with orchestration and transformation  
- Data consolidation in MySQL cluster  
- Web-based BI services with ~2h data freshness  
- Automated reporting engine (CSV, consolidated reports)  

---