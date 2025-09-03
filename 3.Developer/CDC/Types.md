# 🔄 Different Types of Change Data Capture (CDC)

CDC techniques vary based on **how changes are captured** from the source system.  
In Databricks (with Delta Lake and DLT), these techniques are implemented differently, but the underlying CDC concepts remain the same.

---

## 📌 1. **Log-Based CDC**
- Captures changes directly from **database transaction logs** (e.g., SQL Server transaction log, Oracle redo logs, MySQL binlog).
- Low latency & efficient because it avoids full scans.
- Commonly implemented via **Debezium, Kafka, GoldenGate, Fivetran, Informatica**.

✅ **Pros:** Near real-time, low overhead  
⚠️ **Cons:** Requires access to DB logs, setup can be complex  

**Example:**  
A change in SQL Server is streamed via Debezium → Kafka → Databricks Autoloader → Delta Table.

---

## 📌 2. **Trigger-Based CDC**
- Uses **triggers or stored procedures** in the source database.  
- On INSERT/UPDATE/DELETE, the trigger writes changes into a CDC table.  

✅ **Pros:** Easier for small databases  
⚠️ **Cons:** Extra load on source DB, not ideal for high-volume  

**Example:**  
SQL trigger writes to `customer_changes` table → Databricks reads this table → merges into Delta.

---

## 📌 3. **Timestamp-Based CDC**
- Relies on **last updated timestamp columns** in the source table (e.g., `last_modified_at`).  
- Each CDC job fetches rows with `last_modified_at > last_processed_time`.

✅ **Pros:** Simple, widely supported  
⚠️ **Cons:** Requires proper timestamp handling, may miss deletes  

**Example in Databricks:**  

```python
last_run = "2025-09-01 00:00:00"
df = spark.read.format("jdbc").option("query", f"""
    SELECT * FROM customers WHERE last_updated > '{last_run}'
""").load()
````

---

## 📌 4. **Diff-Based CDC (Snapshot Comparison)**

* Compares two full dataset snapshots (current vs previous) and identifies changes.
* Works when no logs or timestamps are available.

✅ **Pros:** Works on any source
⚠️ **Cons:** Expensive, not real-time

**Example:**
Databricks compares yesterday’s file vs today’s file → identifies insert/update/delete → merges into Delta.

---

## 📌 5. **Merge-Based CDC (Delta Lake Specific)**

* Uses **Delta Lake MERGE INTO** for UPSERTS and DELETES.
* Efficient for applying CDC records (from any source).

```python
deltaTable.alias("t") \
  .merge(changes.alias("s"), "t.id = s.id") \
  .whenMatchedUpdateAll() \
  .whenNotMatchedInsertAll() \
  .whenMatchedDelete(condition="s.op = 'DELETE'") \
  .execute()
```

---

## 📌 6. **Streaming CDC**

* Captures changes continuously from sources like **Kafka, Event Hubs, Kinesis**.
* Autoloader + Structured Streaming processes CDC events in near real-time.

✅ **Pros:** Low latency
⚠️ **Cons:** Requires streaming infra

**Example (Streaming Insert/Update/Delete):**

```python
df = (spark.readStream
      .format("cloudFiles")
      .option("cloudFiles.format", "json")
      .load("/mnt/cdc"))
```

---

## 📌 7. **Declarative CDC with DLT (APPLY CHANGES INTO)**

* Simplest CDC in **Delta Live Tables (DLT)** using declarative SQL.
* Handles inserts, updates, deletes automatically with **sequence ordering**.

```sql
APPLY CHANGES INTO LIVE.customer_silver
FROM STREAM(LIVE.customer_bronze)
KEYS (customer_id)
SEQUENCE BY event_time
APPLY AS DELETE WHEN operation = "DELETE";
```

✅ **Pros:** No coding overhead, managed by Databricks
⚠️ **Cons:** Works only in DLT pipelines

---

# 📝 Summary Table – Types of CDC

| CDC Type            | Source Mechanism         | Pros 🚀              | Cons ⚠️             | Best Use Case           |
| ------------------- | ------------------------ | -------------------- | ------------------- | ----------------------- |
| **Log-Based**       | DB transaction logs      | Real-time, efficient | Needs DB log access | Enterprise-scale CDC    |
| **Trigger-Based**   | DB triggers/stored procs | Easy for small DBs   | Adds load           | Small OLTP apps         |
| **Timestamp-Based** | Last updated column      | Simple, common       | Misses deletes      | Slowly changing data    |
| **Diff-Based**      | Snapshot comparison      | Works everywhere     | Expensive           | Legacy systems          |
| **Merge-Based**     | Delta MERGE INTO         | ACID transactions    | Batch only          | Databricks ETL          |
| **Streaming CDC**   | Kafka/Event Hubs         | Low latency          | Infra heavy         | Real-time pipelines     |
| **DLT Declarative** | APPLY CHANGES INTO       | Simplest, managed    | Only in DLT         | Lakehouse CDC pipelines |

---

✅ In **Databricks**, the most common CDC implementations are:

* **MERGE INTO** (batch upserts)
* **DLT APPLY CHANGES INTO** (streaming/declarative)
* **Autoloader + Kafka/Event Hubs** (streaming ingestion)

---
