# Tata Power Solar E‑Torque ETL 

##  Introduction
This project implements a scalable AWS Glue Visual ETL pipeline to process solar torque machine transaction data stored in Amazon S3.  
The pipeline automates ingestion, schema conversion, deduplication, GPS ID generation, and historical tracking using SCD Type 2.  
Processed data is stored in Iceberg tables and queried via Amazon Athena for reporting.

---

## Architecture
![Pipeline Flow](https://github.com/Navyavn06/tata-power-solar-etl/blob/86e79c61b32a9c109a15ddd29029efb27aecb2fe/docs/Tata%20Power%20E-torque%20Architecture.png)

**Flow Summary:**
- **Source** → CSV files in Amazon S3  
- **Transformations** → Schema conversion, GPS ID generation (DENSE_RANK), deduplication (ROW_NUMBER), max bolt count selection  
- **Targets** →  
  - Geo Site Table → unique GPS IDs for locations  
  - Staging Table → validated records for debugging  
  - Transaction Table → active + historical records with SCD Type 2  

## Tech Stack
- AWS Glue Visual ETL  
- Amazon S3  
- AWS Glue Data Catalog  
- Apache Iceberg  
- Amazon Athena  
- SQL (DENSE_RANK, ROW_NUMBER)  



## Data Model
- **Geo Site Table** → Stores unique GPS coordinates with generated GPS_ID  
- **Staging Table** → Holds validated, deduplicated records before production load  
- **Transaction Table** → Maintains active + historical records using SCD Type 2  


## ETL Pipeline Steps
1. Read CSV files from S3  
2. Convert schema (string → int/double/timestamp)  
3. Generate GPS_ID using DENSE_RANK()  
4. Filter records where status = RUN  
5. Deduplicate with ROW_NUMBER() → select max bolt_count per location  
6. Load into Geo Site, Staging, and Transaction tables  
7. Apply SCD Type 2 for historical tracking  
8. Query processed data via Athena  


## Outcomes
- Automated end‑to‑end ETL pipeline  
- Reduced reporting latency from **6h → 2h**  
- Improved data quality with validation + deduplication  
- Preserved historical records using SCD Type 2  
- Enabled efficient reporting via Athena  


