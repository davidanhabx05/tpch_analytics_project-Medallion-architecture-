# tpch_analytics_project-Medallion-architecture-
This repository applies an analytical system that using template data in Snowflake dataware house in big data



┌─────────────────────────────────────────────┐
│              📁 DATA FILES                    │
│   (orders.csv, customers.csv, lineitem.csv…) │
└───────────────────────┬───────────────────────┘
                         │
                         │  [Upload Files]
                         ▼
                ┌──────────────────┐
                │   🗄 STAGE         │
                │ (TPCH_DATA_STAGE) │
                └─────────┬─────────┘
                          │
                          │  [COPY INTO / Snowpipe]
                          ▼
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  🟤 BRONZE LAYER (Raw Data) — STAGING Schema                      ┃
┃    • ORDERS                                                       ┃
┃    • CUSTOMER                                                     ┃
┃    • LINEITEM                                                     ┃
┃    • PART, SUPPLIER, PARTSUPP, NATION, REGION                     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
                                    │
                                    │  🔄 TASKS (TASK_BRONZE_TO_SILVER_*)
                                    │  [Stored Procedures Transform Data]
                                    ▼
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  🥈 SILVER LAYER (Cleaned & Enriched) — SILVER Schema             ┃
┃    • ORDERS_SILVER    (+ status desc, date parts, clerk ID)      ┃
┃    • CUSTOMER_SILVER  (+ nation name, region name)                ┃
┃    • LINEITEM_SILVER  (+ part name, supplier, net price)          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
                                    │
                                    │  🔄 TASKS (TASK_SILVER_TO_GOLD_*)
                                    │  [Calculate Business Metrics]
                                    ▼
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  🥇 GOLD LAYER (Business Metrics) — GOLD Schema                   ┃
┃    • CUSTOMER_LTV          (Customer Lifetime Value)              ┃
┃    • DAILY_SALES_SUMMARY   (Daily KPIs)                           ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
                                    │
                                    ▼
                     📊 REPORTS & DASHBOARDS

--Data file has various of file such as parquet,flat csv, api --in project using flat file csv
--Stage is a place to keep file before pushing it into Bronze layer
--Task <Stored Proceduree: data transformation from Bronzze -> Silver 
--Data is transformed and cleanned from Bronze -> Layer
--Connecting to Power BI with Snowflake data platform
