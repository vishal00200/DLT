+-----------------------------------------------------------------------------------------------------------------+
|                                          RAW DATA SOURCES                                                       |
|                     (Cloud Storage: ADLS Gen2, S3, GCS | Streaming: Kafka, Event Hubs | Databases)              |
+-----------------------------------------------------------------------------------------------------------------+
          |
          |  (Continuous Ingestion - often via Auto Loader)
          V
+-----------------------------------------------------------------------------------------------------------------+
|                                     DATABRICKS DELTA LIVE TABLES (DLT) PIPELINE                                 |
|-----------------------------------------------------------------------------------------------------------------|
|  YOUR DECLARATIVE CODE (Python or SQL Notebooks):                                                               |
|  - `CREATE LIVE TABLE` / `@dlt.table` : Define tables based on transformations.                                 |
|  - `APPLY CHANGES INTO` : Define CDC (Change Data Capture) logic for updates/deletes/merges.                    |
|  - `EXPECT` clauses : Define data quality rules (e.g., `EXPECT (col IS NOT NULL) ON VIOLATION DROP ROW`).      |
|                                                                                                                 |
|  DLT AUTOMATICALLY HANDLES (The "Magic" Behind the Scenes):                                                     |
|  - **Infrastructure Management:** Cluster provisioning, scaling (up/down/out), termination.                     |
|  - **Orchestration & Dependencies:** Automatically builds DAG (Directed Acyclic Graph) of tables, manages task |
|    execution order, retries, and error handling.                                                                |
|  - **Incremental Processing:** Efficiently processes only new or changed data.                                  |
|  - **Data Quality Enforcement:** Applies `EXPECT` rules, logs violations, and can drop/quarantine bad records.  |
|  - **Automatic Lineage:** Generates a visual graph of table dependencies and data flow.                          |
|  - **Monitoring & Alerting:** Provides built-in dashboards for pipeline health, data quality, and performance.  |
|  - **Performance Optimization:** Automatically runs `OPTIMIZE` and `VACUUM` on Delta tables.                    |
|  - **Schema Evolution:** Handles schema changes gracefully.                                                      |
+-----------------------------------------------------------------------------------------------------------------+
          |
          |  (Data Flow & Transformations)
          V
+-----------------------------------------------------------------------------------------------------------------+
|                                             MEDALLION ARCHITECTURE LAYERS                                       |
|-----------------------------------------------------------------------------------------------------------------|
|  +---------------------+   +-----------------------+   +-----------------------+                               |
|  |   BRONZE LAYER      |-->|    SILVER LAYER       |-->|    GOLD LAYER         |                               |
|  | (Raw, Untouched     |   | (Cleaned, Transformed,|   | (Aggregated, Summarized,|                              |
|  |  Delta Tables)      |   |  Enriched Delta Tables)|   |  Business-Ready Tables)|                              |
|  +---------------------+   +-----------------------+   +-----------------------+                               |
|                                                                                                                 |
|  (Each layer is a Delta Table, managed and optimized by DLT)                                                    |
+-----------------------------------------------------------------------------------------------------------------+
          |
          |  (Consumption & Insights)
          V
+-----------------------------------------------------------------------------------------------------------------+
|                                           DOWNSTREAM CONSUMERS                                                  |
|                  (BI Tools, ML Models, Data Scientists, Analysts, Other Applications)                             |
+-----------------------------------------------------------------------------------------------------------------+
