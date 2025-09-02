# ⚡ Autoloader in Databricks  

Databricks **Autoloader** is a feature that enables **incremental and efficient data ingestion** from cloud storage (AWS S3, Azure Data Lake, GCS) into **Delta Lake**.  
It **automatically detects and processes new files** as they arrive, without needing to reprocess older data.  

---

## 🚀 Key Features
- ✅ **Incremental loading** – Only new files are processed.  
- ✅ **Schema evolution** – Handles schema changes automatically.  
- ✅ **Scalability** – Efficient for millions of files.  
- ✅ **Reliability** – Tracks processed files using **checkpoints**.  
- ✅ **Integration** – Works with **structured streaming** for real-time ingestion.  

---

## 🏗️ How Autoloader Works
1. **Cloud Files Source** → Monitors a directory in cloud storage.  
2. **Checkpoints** → Tracks which files have already been ingested.  
3. **Schema Evolution** → Adapts to new columns if schema changes.  
4. **Delta Lake Sink** → Writes data into Delta tables for analytics.  

---

## 🔑 Syntax
```python
df = (spark.readStream
      .format("cloudFiles")                # Autoloader
      .option("cloudFiles.format", "json") # Input format (json, csv, parquet, etc.)
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema") # schema tracking
      .load("/mnt/raw_data/"))             # source folder
````

---

## 📥 Example: Load JSON files into Delta Table

```python
# Read incremental data using Autoloader
df = (spark.readStream
      .format("cloudFiles")
      .option("cloudFiles.format", "json")
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema")
      .load("/mnt/raw/sales"))

# Write into Delta Table
(df.writeStream
   .format("delta")
   .option("checkpointLocation", "/mnt/checkpoints/sales")
   .outputMode("append")
   .table("bronze_sales"))
```

---

## 🗂️ Schema Evolution Example

```python
df = (spark.readStream
      .format("cloudFiles")
      .option("cloudFiles.format", "csv")
      .option("cloudFiles.inferColumnTypes", "true") # auto infer schema
      .option("cloudFiles.schemaEvolutionMode", "addNewColumns")
      .option("cloudFiles.schemaLocation", "/mnt/checkpoints/schema")
      .load("/mnt/raw/customers"))
```

👉 If a new column appears in future files, Autoloader will **automatically add it**.

---

## ⚖️ Two Modes of File Discovery

| Mode                  | Description                                                          | Best Use Case                             |
| --------------------- | -------------------------------------------------------------------- | ----------------------------------------- |
| **Directory Listing** | Scans directory for new files.                                       | Few files, small datasets.                |
| **File Notification** | Uses cloud-native event services (S3, ADLS, GCS) for file discovery. | Millions of files, large-scale ingestion. |

---

## 🏛️ Autoloader in the Lakehouse

* **Bronze Layer** → Ingest raw data incrementally.
* **Silver Layer** → Apply transformations, cleansing.
* **Gold Layer** → Aggregated, business-ready data.

---

## 🎯 Benefits of Autoloader

* No need for **manual scheduling** of ingestion jobs.
* Saves **compute costs** by avoiding reprocessing.
* Provides **real-time data availability**.
* Handles **schema drift** with minimal effort.

---

🔹 **In summary:**
Autoloader = *Smart, scalable, and incremental data ingestion into Delta Lake with minimal configuration*.