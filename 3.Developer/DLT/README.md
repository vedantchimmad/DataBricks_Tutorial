# ⚡ Delta Live Tables (DLT) in Databricks

Delta Live Tables (**DLT**) is a **framework for building reliable, maintainable, and declarative ETL pipelines** in Databricks.  
It builds on top of **Delta Lake**, providing automation for **data ingestion, transformation, validation, and governance**.

---

## 🔹 What is Delta Live Tables?

- A **pipeline framework** to define data transformations in **SQL or Python**.  
- Automatically manages **dependencies**, **data quality**, **schema evolution**, and **error handling**.  
- Ensures **data is always fresh, reliable, and production-ready**.  
- Provides **observability** with monitoring, lineage tracking, and metrics.  

---

## 🔹 DLT Architecture (Simplified)

```text
                +-------------------+
                |   Data Sources    |
                +-------------------+
                         |
                         v
                +-------------------+
                |   DLT Pipelines   |
                | (SQL / Python)    |
                +-------------------+
                   |   |   |   |
      +------------+   |   |   +-----------+
      v                v   v               v
+-----------+    +-----------+     +---------------+
| Bronze    |    | Silver    |     |   Gold        |
| Raw Data  |    | Cleansed  |     | Business Data |
+-----------+    +-----------+     +---------------+
                         |
                         v
                +-------------------+
                |  BI / ML / Apps   |
                +-------------------+
````

---

## 🔹 Key Features

- ✅ **Declarative pipelines** – Define *what* you want, not *how*.
- ✅ **Multiple languages** – Write transformations in **SQL or Python**.
- ✅ **Data quality (Expectations)** – Validate incoming data with `EXPECT`.
- ✅ **Auto-optimization** – Handles **checkpointing, retries, scaling** automatically.
- ✅ **Incremental processing** – Reads only new/changed data (CDC ready).
- ✅ **Built-in lineage** – Visual DAG of pipeline stages.
- ✅ **Governance** – Fully integrated with **Unity Catalog & Delta Lake**.

---

## 🔹 DLT Concepts

| Concept               | Description                                                       |
| --------------------- | ----------------------------------------------------------------- |
| **Pipeline**          | The DLT workflow, consisting of transformations and expectations. |
| **Table**             | Managed Delta table created by a DLT pipeline.                    |
| **Streaming Table**   | A continuously updating table (ingests real-time data).           |
| **Materialized View** | Batch-updated view managed by DLT.                                |
| **Expectations**      | Data quality checks applied during ingestion/transformation.      |

---

## 🔹 Example – SQL DLT Pipeline

```sql
-- Bronze Table: Raw data ingestion
CREATE OR REFRESH STREAMING LIVE TABLE bronze_events
COMMENT "Raw events from JSON logs"
AS SELECT * FROM cloud_files("/mnt/raw/events", "json");

-- Silver Table: Cleaned data
CREATE OR REFRESH LIVE TABLE silver_events
COMMENT "Cleansed and filtered events"
AS
SELECT id, userId, eventType, eventTime
FROM LIVE.bronze_events
WHERE eventType IS NOT NULL;

-- Gold Table: Aggregated business data
CREATE OR REFRESH LIVE TABLE gold_event_summary
COMMENT "Business summary of events"
AS
SELECT userId, COUNT(*) AS event_count
FROM LIVE.silver_events
GROUP BY userId;
```

---

## 🔹 Example – Python DLT Pipeline

```python
import dlt
from pyspark.sql.functions import col

@dlt.table(
    comment="Raw events table from JSON source"
)
def bronze_events():
    return spark.readStream.format("json").load("/mnt/raw/events")

@dlt.table(
    comment="Cleaned events with filters"
)
def silver_events():
    return dlt.read_stream("bronze_events").filter(col("eventType").isNotNull())

@dlt.table(
    comment="Aggregated business-level metrics"
)
def gold_event_summary():
    return (dlt.read("silver_events")
            .groupBy("userId")
            .count())
```

---

## 🔹 Data Quality (Expectations)

DLT allows **built-in data quality enforcement**:

```sql
CREATE OR REFRESH LIVE TABLE clean_events
TBLPROPERTIES ("quality" = "silver")
AS
SELECT *
FROM LIVE.raw_events
EXPECT id IS NOT NULL ON VIOLATION DROP ROW
EXPECT eventType IN ('click', 'purchase') ON VIOLATION FAIL UPDATE;
```

* `DROP ROW` → removes invalid records.
* `FAIL UPDATE` → stops pipeline if expectation fails.

---

## 🔹 Benefits of DLT

* 🚀 **Simpler pipelines** → less code, declarative style.
* 🔄 **Handles orchestration** → retries, dependencies, scheduling.
* 🛡️ **Built-in quality checks** with expectations.
* 📊 **Automatic lineage tracking** (UI visualization).
* ⚡ **Scales seamlessly** with Databricks compute.

---

## 🔹 When to Use DLT?

* Streaming or batch **ETL pipelines**.
* When **data reliability** and **lineage tracking** are critical.
* To **enforce data quality rules** at ingestion.
* For **incremental CDC pipelines**.

---

👉 DLT is essentially the **next-gen ETL framework** inside Databricks, replacing manual Spark jobs with a **declarative, quality-first approach**.
