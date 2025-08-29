# 🌊 Delta Lake in Databricks  

Delta Lake is an **open-source storage layer** that brings **reliability, performance, and ACID transactions** to data lakes.  
It is tightly integrated with Databricks and enables the **Lakehouse architecture**.  

---

## 🏗️ What is Delta Lake?  

- ✅ Built on top of **Apache Parquet** files  
- ✅ Ensures **ACID transactions** (Atomicity, Consistency, Isolation, Durability)  
- ✅ Provides **schema enforcement & evolution**  
- ✅ Enables **time travel** (querying older versions of data)  
- ✅ Supports **unified batch & streaming**  

---

## 🔑 Key Features  

| Feature | Description | Example |
|---------|-------------|---------|
| **ACID Transactions** | Guarantees reliable writes & reads | Multiple users can read/write safely |
| **Schema Enforcement** | Prevents bad/incorrect data | Rejects invalid schema writes |
| **Schema Evolution** | Allows schema updates | Add new columns dynamically |
| **Time Travel** | Query old versions of data | `VERSION AS OF` |
| **Scalable Metadata** | Stores metadata in Delta logs | Faster than Hive Metastore |
| **Batch + Streaming** | Supports both workloads | Structured Streaming with Delta |

---

## 📊 Delta Lake Architecture  

```
         +-----------------------------+
         |       Delta Lake Table       |
         +-----------------------------+
         | Parquet Files (Data Storage) |
         | Delta Log (_delta_log JSON)  |
         +-----------------------------+
```
```
📁 /mnt/delta/events
├── part-0001.snappy.parquet <- Data File
├── part-0002.snappy.parquet <- Data File
├── _delta_log/ <- Transaction Log
│ ├── 00000000000000000001.json <- Transaction #1
│ ├── 00000000000000000002.json <- Transaction #2
│ ├── 00000000000000000003.checkpoint.parquet
│ └── ...
└── ...
```

- **Parquet files** → Store the actual data  
- **Delta Log (`_delta_log`)** → Stores metadata, schema changes, and transactions  

---

## ⚙️ Delta Lake Commands  

### 1. **Create a Delta Table**
```python
data = spark.range(0, 5)
data.write.format("delta").save("/mnt/delta/events")
````

---

### 2. **Read a Delta Table**

```python
df = spark.read.format("delta").load("/mnt/delta/events")
df.show()
```

---

### 3. **Update Data**

```python
from delta.tables import DeltaTable

deltaTable = DeltaTable.forPath(spark, "/mnt/delta/events")
deltaTable.update(
    condition = "id = 2",
    set = { "id": "200" }
)
```

---

### 4. **Delete Data**

```python
deltaTable.delete("id = 3")
```

---

### 5. **Merge (Upsert)**

```python
newData = spark.createDataFrame([(2,), (5,)], ["id"])

deltaTable.alias("old").merge(
    newData.alias("new"),
    "old.id = new.id"
).whenMatchedUpdate(set = { "id": "new.id" }) \
 .whenNotMatchedInsert(values = { "id": "new.id" }) \
 .execute()
```

---

### 6. **Time Travel**

```python
# Query by version
df = spark.read.format("delta").option("versionAsOf", 1).load("/mnt/delta/events")

# Query by timestamp
df = spark.read.format("delta").option("timestampAsOf", "2025-08-25 12:00:00").load("/mnt/delta/events")
```

---

## 🚀 Benefits of Delta Lake in Databricks

* 🔹 **Data Reliability** → Handles concurrent reads/writes safely
* 🔹 **Faster Queries** → Optimized storage & caching
* 🔹 **Cost-Effective** → Store raw + curated data in the same place
* 🔹 **Governance Ready** → Works with **Unity Catalog** for access control
* 🔹 **Streaming + Batch Together** → No need for separate architectures

---

## 🏆 Use Cases

* 📂 **Data Lakehouse** (Unified Data Lake + Warehouse)
* 📈 **ETL Pipelines** (Incremental batch processing)
* ⏳ **Historical Analysis** (using Time Travel)
* ⚡ **Real-Time Analytics** (streaming with Delta)
* 🔍 **ML Feature Store** (storing curated ML features)

---

✅ Delta Lake is the **foundation of Databricks Lakehouse**: it combines the **scalability of data lakes** with the **performance of warehouses**.

