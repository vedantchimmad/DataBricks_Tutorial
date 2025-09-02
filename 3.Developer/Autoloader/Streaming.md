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

