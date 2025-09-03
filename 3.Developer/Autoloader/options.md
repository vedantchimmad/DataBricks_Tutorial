# ⚡ Databricks Auto Loader – All Options Explained

Databricks **Auto Loader** has many configuration options grouped into categories:  
**File format, Schema, Discovery, Metadata, Error handling, and Performance.**  
Below is a detailed explanation of every available option with examples.

---

## 🔹 1. File Source & Format Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.format` | Format of incoming files. | `"json"`, `"csv"`, `"parquet"`, `"avro"`, `"orc"`, `"binaryFile"` | Defines how to parse ingested data. |
| `cloudFiles.includeExistingFiles` | Whether to process files that already exist in the directory before the stream starts. | `"true"` or `"false"` | Useful for backfilling data. |
| `cloudFiles.resourceGroup` | (Azure only) Limit resources for Auto Loader within a resource group. | `"rg-data"` | Helps control costs in Azure. |
| `cloudFiles.region` | Region where notification services will be created. | `"eastus"`, `"us-west-2"` | Needed when using notifications instead of listing. |

---

## 🔹 2. Schema Management Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.schemaLocation` | **Required**. Path to store schema checkpoints. | `"/mnt/schema/events"` | Ensures schema consistency across batches. |
| `cloudFiles.inferColumnTypes` | Infer numeric and boolean column types. | `"true"` | By default, all inferred types are strings. |
| `cloudFiles.schemaHints` | Provide explicit column types. | `"id INT, name STRING"` | Prevents wrong inference. |
| `cloudFiles.schemaEvolutionMode` | Controls schema changes. | `"addNewColumns"`, `"rescue"` | - `addNewColumns`: Adds new columns. <br> - `rescue`: Unexpected fields go to `_rescued_data`. |
| `cloudFiles.allowOverwrites` | Allow overwriting schema if already detected. | `"true"` or `"false"` | By default, overwrite is not allowed. |

---

## 🔹 3. File Discovery Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.useNotifications` | Enable cloud-native notifications (S3, ADLS, GCS). | `"true"` | Faster than directory listing. |
| `cloudFiles.useIncrementalListing` | Incrementally track new files with listing. | `"true"` | Reduces cost of listing in very large directories. |
| `cloudFiles.backfillInterval` | Frequency of re-checking for missing older files. | `"1d"` | Helps catch late-arriving files. |
| `cloudFiles.maxFilesPerTrigger` | Limit number of files processed per trigger. | `"1000"` | Prevents overwhelming cluster. |
| `cloudFiles.maxBytesPerTrigger` | Limit data size processed per trigger. | `"1g"` | Controls batch size by data volume. |

---

## 🔹 4. File Metadata Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.partitionColumns` | Partition data by column(s). | `["year","month"]` | Organizes data into Hive-style partitions. |
| `cloudFiles.includeFileName` | Add input file name as a new column. | `"true"` | Useful for tracing source of data. |
| `cloudFiles.includeFilePath` | Add file path as a new column. | `"true"` | Useful when multiple sources are ingested. |
| `cloudFiles.includeFileModificationTime` | Add last modified timestamp. | `"true"` | Useful for data lineage and auditing. |

---

## 🔹 5. Error Handling & Quality Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.maxFileAge` | Ignore files older than threshold. | `"7d"` | Useful for ignoring stale files. |
| `cloudFiles.ignoreCorruptFiles` | Skip corrupt/unreadable files. | `"true"` | Prevents job failures. |
| `cloudFiles.allowOverwrites` | If a file with the same name arrives, overwrite data. | `"false"` | By default, duplicates are ignored. |

---

## 🔹 6. Performance Optimization Options

| Option | Description | Example | Explanation |
|--------|-------------|---------|-------------|
| `cloudFiles.useIncrementalListing` | Use incremental listing to reduce directory scan cost. | `"true"` | Saves cost in cloud storage with millions of files. |
| `cloudFiles.useNotifications` | Use cloud-native notifications (S3 events, ADLS Event Grid, GCS Pub/Sub). | `"true"` | Best for **low-latency ingestion**. |
| `cloudFiles.maxFilesPerTrigger` | Cap number of files processed per microbatch. | `"500"` | Avoids processing spikes. |
| `cloudFiles.maxBytesPerTrigger` | Cap total size processed per batch. | `"500m"` | Useful when files vary in size. |

---

## 🔹 7. Example Configurations

### ✅ JSON ingestion with schema evolution
```python
df = (spark.readStream.format("cloudFiles")
      .option("cloudFiles.format", "json")
      .option("cloudFiles.schemaLocation", "/mnt/schema/events")
      .option("cloudFiles.inferColumnTypes", "true")
      .option("cloudFiles.schemaEvolutionMode", "addNewColumns")
      .load("/mnt/raw/events"))
````

### ✅ CSV ingestion with notifications and limits

```python
df = (spark.readStream.format("cloudFiles")
      .option("cloudFiles.format", "csv")
      .option("cloudFiles.schemaLocation", "/mnt/schema/csv")
      .option("cloudFiles.useNotifications", "true")
      .option("cloudFiles.maxFilesPerTrigger", "200")
      .option("cloudFiles.ignoreCorruptFiles", "true")
      .load("/mnt/raw/csv_data"))
```

---

## ✅ Key Takeaways

1. **`cloudFiles.schemaLocation` is mandatory** for schema inference & evolution.
2. Use **`cloudFiles.useNotifications`** for low-latency ingestion at scale.
3. Control performance using **`maxFilesPerTrigger`** and **`maxBytesPerTrigger`**.
4. Schema changes can be handled using **`addNewColumns`** or **`rescue` mode**.
5. Metadata options (`includeFileName`, `includeFilePath`) are very useful for lineage & debugging.

---

⚡ Auto Loader is highly configurable → designed for **scalable, schema-aware, low-cost data ingestion**.

