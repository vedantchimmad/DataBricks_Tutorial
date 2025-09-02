# ⚡ Autoloader Streaming in Databricks  

Databricks **Autoloader** works best when used with **Structured Streaming**, allowing **real-time or near real-time ingestion** of data from cloud storage (AWS S3, ADLS, GCS) into **Delta Lake**.  

It continuously monitors the source directory for new files and loads them into a **streaming DataFrame** without reprocessing old files.  

---

## 🏗️ How Autoloader Streaming Works
1. **Cloud Storage →** Watches a directory for new incoming files.  
2. **Autoloader (cloudFiles) →** Streams only **new data** into Spark.  
3. **Checkpointing →** Keeps track of processed files for fault tolerance.  
4. **Delta Sink →** Writes data into a Delta table (Bronze → Silver → Gold layers).  

---

## 🔑 Basic Streaming Example
```python
# Read streaming data with Autoloader
df = (spark.readStream
      .format("cloudFiles")                      # Autoloader
      .option("cloudFiles.format", "json")       # File format
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema") 
      .load("/mnt/raw/sales"))                   # Source path

# Write stream to Delta table
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .outputMode("append")
   .table("bronze_sales"))
````

👉 This continuously loads new files from `/mnt/raw/sales` into `bronze_sales`.

---

## 🗂️ Streaming with Transformation (Bronze → Silver)

```python
# Bronze Layer: Raw ingestion
bronze_df = (spark.readStream
             .format("cloudFiles")
             .option("cloudFiles.format", "csv")
             .option("cloudFiles.schemaLocation", "/mnt/checkpoints/bronze_schema")
             .load("/mnt/raw/customers"))

# Silver Layer: Apply transformations
silver_df = bronze_df.selectExpr("id", "name", "UPPER(city) as city", "ingest_time")

# Write to Delta Silver table
(silver_df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/silver")
   .outputMode("append")
   .table("silver_customers"))
```

👉 Data flows **continuously**: Raw files → Bronze → Silver.

---

## ⚙️ Important Options

| Option                           | Purpose                                                     |
| -------------------------------- | ----------------------------------------------------------- |
| `cloudFiles.format`              | File format (`json`, `csv`, `parquet`, etc.)                |
| `cloudFiles.schemaLocation`      | Directory to store inferred schema & track schema evolution |
| `cloudFiles.inferColumnTypes`    | Automatically detect column types (`true/false`)            |
| `cloudFiles.schemaEvolutionMode` | Handle schema drift (`addNewColumns`, `rescue`)             |
| `checkpointLocation`             | Ensures fault tolerance and exactly-once ingestion          |

---

## 🔁 Streaming Modes

* **Append** → Adds new rows (default, used for files).
* **Complete** → Writes entire result each trigger (not common for files).
* **Update** → Writes only updated rows (used with aggregations).

---

## 📊 Example: Real-Time Aggregation with Autoloader

```python
# Read JSON files as stream
df = (spark.readStream
      .format("cloudFiles")
      .option("cloudFiles.format", "json")
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema")
      .load("/mnt/raw/transactions"))

# Aggregate streaming data
agg_df = df.groupBy("product_id").count()

# Write aggregation to Delta
(agg_df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/agg")
   .outputMode("complete")
   .table("product_counts"))
```

---

## 🏛️ Autoloader Streaming in Lakehouse

🔹 **Bronze Layer (Raw)** → Store raw data as-is.
🔹 **Silver Layer (Cleansed)** → Apply schema, transformations, and joins.
🔹 **Gold Layer (Aggregated)** → Business-ready tables for analytics.

📌 **Example flow:**
`Cloud Storage (Raw JSON/CSV)` → `Autoloader Streaming` → `Delta Bronze` → `Delta Silver` → `Delta Gold`

---

## 🎯 Benefits of Autoloader Streaming

* ✅ Real-time ingestion with low latency.
* ✅ Handles schema drift automatically.
* ✅ Reliable with checkpointing (exactly-once semantics).
* ✅ Scales to **millions of files**.
* ✅ Integrates with **Delta Lake time travel, ACID transactions, and ML pipelines**.

---

🔥 **In short:**
**Autoloader Streaming = Real-time, fault-tolerant, scalable ingestion pipeline for cloud data → Delta Lake.**
---
## ⏱️ Trigger Interval in Autoloader Streaming (Databricks)

In **Structured Streaming with Autoloader**, the **trigger interval** controls **how often Spark checks for new data** and processes it.  
It’s defined using `.trigger()` in the write stream.

---

## ⚙️ Types of Triggers

### 1️⃣ **Default (Micro-batch Mode)**
If no trigger is set, Spark processes **new files as soon as they arrive**.
```python
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .table("bronze_sales"))
````

👉 Processes continuously (every micro-batch).

---

### 2️⃣ **Fixed Interval Trigger**

Run the stream at **regular intervals** (e.g., every 1 minute).

```python
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .trigger(processingTime="1 minute")
   .table("bronze_sales"))
```

🔹 Spark will check the source every **1 min** for new files.

---

### 3️⃣ **Once Trigger**

Runs the query **only once**, processing all available data, then stops.

```python
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .trigger(once=True)
   .table("bronze_sales"))
```

👉 Useful for **batch-like runs** with streaming syntax.

---

### 4️⃣ **Available-Now Trigger** (Near-Batch Mode)

Processes **all available data immediately** and then stops (similar to once, but optimized).

```python
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .trigger(availableNow=True)
   .table("bronze_sales"))
```

🔹 Often used for **catch-up ingestion** (historical data load).

---

## 📊 When to Use Which?

| Trigger Type              | Use Case                                                                |
| ------------------------- | ----------------------------------------------------------------------- |
| **Default (micro-batch)** | Real-time streaming (low latency).                                      |
| **Fixed interval**        | When you want predictable **scheduled ingestion** (e.g., every 5 mins). |
| **Once**                  | For batch jobs that process data **only once** and exit.                |
| **AvailableNow**          | Best for backfilling historical data without running continuously.      |

---

## 🛠️ Example: Streaming with Trigger Interval

```python
df = (spark.readStream
      .format("cloudFiles")
      .option("cloudFiles.format", "json")
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema")
      .load("/mnt/raw/transactions"))

# Write with trigger every 2 minutes
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/transactions")
   .trigger(processingTime="2 minutes")
   .table("bronze_transactions"))
```

👉 This stream **checks for new files every 2 minutes**.

---

✅ **Summary**

* `trigger(processingTime="X minutes")` → Schedules micro-batches.
* `trigger(once=True)` → Single batch & stop.
* `trigger(availableNow=True)` → Ingest all available data & stop.

---

🔥 Pro Tip: For **low-latency pipelines** (IoT, logs), use **micro-batch (default)**.
For **cost-efficient ingestion** of large files, use **fixed intervals or availableNow**.
---
## 📤 Output Modes in Delta Lake Streaming (Databricks)

When using **Structured Streaming** with Databricks (Autoloader, Kafka, files, etc.),  
the **output mode** defines **what part of the result table gets written** to the sink (Delta, console, Kafka, etc.).

---

## ⚙️ Types of Output Modes

### 1️⃣ **Append Mode**
- Only **new rows** added to the result table since the last trigger are written.
- 🚀 Most commonly used with **immutable sinks** (like Delta Lake, Kafka, or files).

```python
query = (df.writeStream
           .format("delta")
           .outputMode("append")
           .option("checkpointLocation", "/mnt/checkpoints/append_demo")
           .table("bronze_sales"))
````

✅ Best for **event data** where new records keep arriving (e.g., logs, transactions).
❌ Cannot be used if the query has **aggregations without watermark**.

---

### 2️⃣ **Complete Mode**

* Writes the **entire result table** to the sink after every trigger.
* ⚡ Overwrites the previous output.

```python
query = (df.writeStream
           .format("console")
           .outputMode("complete")
           .start())
```

✅ Best for **aggregations** (like counts, averages, group by).
⚠️ Works only for **queries with aggregations**.
❌ Not supported for file sinks (e.g., Delta, Parquet).

---

### 3️⃣ **Update Mode**

* Only the **updated rows** (new + changed) in the result table since last trigger are written.

```python
query = (df.writeStream
           .format("delta")
           .outputMode("update")
           .option("checkpointLocation", "/mnt/checkpoints/update_demo")
           .table("silver_sales"))
```

✅ More efficient than `complete` because it writes **only changed rows**.
✅ Works well for **stateful aggregations** (e.g., running totals with watermark).
❌ Not supported for all sinks (like file sinks in some cases).

---

## 📊 Comparison

| Output Mode  | What is written?                     | Use Case                             | Supported Sinks       |
| ------------ | ------------------------------------ | ------------------------------------ | --------------------- |
| **Append**   | Only **new rows** since last trigger | Event logs, transactions, CDC        | Delta, Kafka, Parquet |
| **Complete** | **All rows** every time              | Aggregations (counts, sums, avg)     | Console, Memory       |
| **Update**   | Only **new + changed rows**          | Stateful aggregations with watermark | Delta, Kafka          |

---

## 🛠️ Example: Different Modes

### 🔹 Append Example

```python
df = spark.readStream.format("cloudFiles") \
    .option("cloudFiles.format", "json") \
    .load("/mnt/raw/transactions")

df.writeStream \
  .format("delta") \
  .outputMode("append") \
  .option("checkpointLocation", "/mnt/checkpoints/append") \
  .table("bronze_transactions")
```

---

### 🔹 Complete Example

```python
agg_df = df.groupBy("product_id").count()

agg_df.writeStream \
  .format("console") \
  .outputMode("complete") \
  .start()
```

---

### 🔹 Update Example

```python
agg_df = df.groupBy("product_id").count()

agg_df.writeStream \
  .format("delta") \
  .outputMode("update") \
  .option("checkpointLocation", "/mnt/checkpoints/update") \
  .table("silver_sales_agg")
```

---

✅ **Summary**

* `append` → Only new rows (best for raw event streams).
* `complete` → Entire result table each trigger (good for aggregations).
* `update` → Only changed rows (efficient for stateful aggregations).

---

🔥 Pro Tip:

* Use **append** for **Bronze layer** (raw ingestion).
* Use **update/complete** for **Silver/Gold layer** (aggregations, transformations).

