
# 🚀 NASA Space Hazard Tracker (Azure Databricks)

This project demonstrates how to build a **data engineering pipeline** on **Azure Databricks** using multiple NASA public APIs from [api.nasa.gov](https://api.nasa.gov/).  
It follows the **Medallion Architecture** (Bronze → Silver → Gold) and leverages **Delta Live Tables (DLT)** for reliable ETL.  

---

## 🌌 APIs Used

- **Asteroids NeoWs** → Near-Earth Object data  
- **DONKI** → Space weather events (solar flares, CMEs, geomagnetic storms)  
- **EONET** → Natural events on Earth (volcanoes, wildfires, storms)  
- **EPIC** → Earth imagery data  

---

## 🏗️ Architecture

```
NASA APIs (JSON)
        │
        ▼
   Bronze Layer (Raw Ingestion)
        │
        ▼
   Silver Layer (Cleansed & Structured)
        │
        ▼
   Gold Layer (Aggregated Insights)
        │
        ▼
 Power BI / Databricks SQL (Visualization)
```

---

## ⚙️ Technologies Used

- **Azure Databricks (Unity Catalog Enabled)**
- **Azure Data Lake Storage (ADLS Gen2)**
- **Delta Lake & Delta Live Tables (DLT)**
- **PySpark**
- **Databricks Workflows (orchestration)**
- **Power BI / Databricks SQL (Analytics)**

---

## 📂 Project Structure

```bash
├── notebooks/
│   ├── bronze/
│   │   ├── 01_ingest_neo_api_bronze.py
│   │   ├── 02_ingest_donki_api_bronze.py
│   │   ├── 03_ingest_eonet_api_bronze.py
│   │   ├── 04_ingest_epic_api_bronze.py
│   │
│   ├── silver/
│   │   ├── 01_transform_neo_silver.py
│   │   ├── 02_transform_donki_silver.py
│   │   ├── 03_transform_eonet_silver.py
│   │   ├── 04_transform_epic_silver.py
│   │
│   ├── gold/
│   │   ├── 01_gold_neo_hazard_summary.py
│   │   ├── 02_gold_donki_event_summary.py
│   │   ├── 03_gold_global_event_summary.py
│   │   ├── 04_gold_visualization_views.py
│
├── dlt_pipelines/
│   ├── bronze_pipeline.json
│   ├── silver_pipeline.json
│   ├── gold_pipeline.json
│
├── dashboards/
│   ├── nasa_hazard_dashboard.pbix
│
└── README.md
```

---

## 🗂️ Unity Catalog Setup

```sql
-- Create Catalog & Schemas
CREATE CATALOG nasa_analytics;

CREATE SCHEMA nasa_analytics.bronze;
CREATE SCHEMA nasa_analytics.silver;
CREATE SCHEMA nasa_analytics.gold;

-- Create External Location for Bronze Layer
CREATE EXTERNAL LOCATION nasa_bronze
URL 'abfss://nasa-analytics@deacourseextdl0070.dfs.core.windows.net/bronze'
WITH (STORAGE CREDENTIAL my_credential);

-- Repeat for Silver and Gold if needed
CREATE EXTERNAL LOCATION nasa_silver
URL 'abfss://nasa-analytics@deacourseextdl0070.dfs.core.windows.net/silver'
WITH (STORAGE CREDENTIAL my_credential);

CREATE EXTERNAL LOCATION nasa_gold
URL 'abfss://nasa-analytics@deacourseextdl0070.dfs.core.windows.net/gold'
WITH (STORAGE CREDENTIAL my_credential);
```

---

## 🛠️ Steps to Run

1. **Configure External Locations (ADLS)**  
   Make sure your storage credentials are attached to Unity Catalog and external locations are created for Bronze, Silver, and Gold layers.

2. **Run Bronze Notebooks**  
   - Ingest raw JSON data from NASA APIs into Bronze tables.  
   - Examples:
     - `01_ingest_neo_api_bronze.py` → NeoWs API  
     - `02_ingest_donki_api_bronze.py` → DONKI API  

3. **Run Silver Notebooks**  
   - Clean, normalize, and flatten the JSON data into structured Silver tables.  
   - Examples:
     - `01_transform_neo_silver.py`  
     - `02_transform_donki_silver.py`  

4. **Run Gold Notebooks**  
   - Aggregate and join Silver tables into Gold tables for analytics-ready datasets.  
   - Examples:
     - `01_gold_neo_hazard_summary.py`  
     - `02_gold_donki_event_summary.py`  
     - `03_gold_global_event_summary.py`  

5. **Deploy Delta Live Table Pipelines**  
   - Create pipelines for Bronze, Silver, and Gold layers.  
   - Configure notebooks in each DLT pipeline.  
   - Run pipelines in **Continuous** or **Triggered** mode.

6. **Visualization**  
   - Connect Power BI or Databricks SQL to Gold tables.  
   - Build dashboards for:
     - Near-Earth asteroid hazards  
     - Solar storms & geomagnetic events  
     - Global natural disasters  
     - Earth imagery snapshots  

---

## 📊 Example Gold Tables

- **`nasa_analytics.gold.neo_hazard_summary`** → Hazardous asteroid statistics  
- **`nasa_analytics.gold.donki_event_summary`** → Solar flares and geomagnetic storm summaries  
- **`nasa_analytics.gold.global_event_summary`** → EONET natural event trends  
- **`nasa_analytics.gold.earth_imagery`** → Latest EPIC Earth images  

---

## 🎯 Outcomes

- End-to-end **ETL pipeline** for space hazard analytics.  
- **Bronze → Silver → Gold Medallion architecture** with Unity Catalog.  
- **Automated ingestion** from multiple NASA APIs using PySpark + DLT.  
- **Unified hazard dashboard** for asteroids, space weather, and Earth events.  

---

🔗 This project demonstrates **real-world data engineering with Azure Databricks** using **NASA Open Data**.
