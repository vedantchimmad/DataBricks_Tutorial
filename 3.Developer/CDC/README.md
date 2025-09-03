# 🔄 Change Data Capture (CDC) in Databricks

**Change Data Capture (CDC)** is the process of identifying and capturing changes (INSERTS, UPDATES, DELETES) made to source data, and applying them to target systems (like a Delta Lake table) **incrementally**, instead of reprocessing the full dataset.

---

## 🔹 Why CDC?

- 🚀 **Efficiency** → Process only changed data instead of full refresh.  
- 🕒 **Low latency** → Near real-time updates to downstream systems.  
- 💾 **Cost savings** → Reduced compute and storage usage.  
- 🔄 **Data consistency** → Keeps target in sync with source.  

---

## 🔹 CDC with Delta Lake in Databricks

Delta Lake provides **ACID transactions, schema evolution, and time travel**, making it perfect for CDC pipelines.  

Two main CDC approaches in Databricks:

1. **Merge-based CDC** – Use Delta Lake’s `MERGE INTO` to apply changes.  
2. **DLT CDC** – Use **Delta Live Tables** with `APPLY CHANGES INTO` for declarative CDC pipelines.  

---

## 🔹 1. Merge-Based CDC (Delta Lake)

### Example – Upserts using `MERGE INTO`

```python
from delta.tables import DeltaTable

# Existing Delta table
delta_table = DeltaTable.forPath(spark, "/mnt/delta/target")

# Incoming changes (from Kafka, files, DB, etc.)
updates_df = spark.read.format("json").load("/mnt/raw/cdc_updates")

# Apply CDC logic
(
  delta_table.alias("t")
  .merge(
    updates_df.alias("s"),
    "t.id = s.id"   # Join condition
  )
  .whenMatchedUpdateAll()      # Update if matched
  .whenNotMatchedInsertAll()   # Insert if not matched
  .execute()
)
````

✅ Handles **inserts and updates**.
You can also handle **deletes** with `.whenMatchedDelete()`.

---

## 🔹 2. DLT CDC with `APPLY CHANGES INTO`

Delta Live Tables (DLT) makes CDC easier with declarative syntax.

### Example – CDC with DLT

```sql
-- Apply CDC changes from source table into target
APPLY CHANGES INTO LIVE.customer_gold
FROM STREAM(LIVE.customer_silver)
KEYS (customer_id)
APPLY AS DELETE WHEN operation = "DELETE"
SEQUENCE BY event_time
COLUMNS * EXCEPT (operation, event_time);
```

* `KEYS (customer_id)` → Defines unique business key.
* `APPLY AS DELETE` → Marks deletes when condition met.
* `SEQUENCE BY event_time` → Ensures correct ordering of updates.
* `COLUMNS * EXCEPT (...)` → Exclude CDC metadata columns.

---

## 🔹 3. CDC from Database Sources

Databricks supports CDC ingestion from:

* **Azure SQL / SQL Server** – via **Azure Data Factory** or **Debezium + Kafka**.
* **Oracle, MySQL, PostgreSQL** – via **Fivetran, Informatica, Confluent**.
* **SAP, Salesforce, other SaaS apps** – via partner connectors.

---

## 🔹 Example CDC Flow (Bronze → Silver → Gold)

1. **Bronze**: Raw CDC events (insert/update/delete).
2. **Silver**: Cleaned CDC data with transformations.
3. **Gold**: Business-level tables with latest state.

```sql
-- Bronze: Raw CDC events
CREATE OR REFRESH STREAMING LIVE TABLE bronze_customers
AS SELECT * FROM cloud_files("/mnt/raw/cdc/customers", "json");

-- Silver: Apply CDC changes
APPLY CHANGES INTO LIVE.silver_customers
FROM STREAM(LIVE.bronze_customers)
KEYS (customer_id)
SEQUENCE BY event_time
APPLY AS DELETE WHEN operation = 'DELETE';

-- Gold: Latest customer state
CREATE OR REFRESH LIVE TABLE gold_customers
AS SELECT * FROM LIVE.silver_customers;
```

---

## 🔹 Benefits of CDC in Databricks

* 🛡️ **Reliable** with Delta Lake’s ACID guarantees.
* ⚡ **Scalable** to billions of rows.
* 🔄 **Handles inserts, updates, and deletes** natively.
* 🔍 **Lineage & observability** with DLT.
* 🧩 **Integrates** with Kafka, Event Hubs, and cloud DBs.

---

## ✅ Key Takeaways

* Use **`MERGE INTO`** for CDC in ad-hoc Spark/Delta pipelines.
* Use **`APPLY CHANGES INTO`** in **Delta Live Tables** for simpler, declarative CDC.
* Combine CDC with **Bronze/Silver/Gold architecture** for reliability.
* Perfect for **real-time data warehousing, data lakehouse, and ML feature stores**.