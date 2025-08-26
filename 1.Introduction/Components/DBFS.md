# 📂 Databricks File System (DBFS)  

---

## 🔹 Introduction  
The **Databricks File System (DBFS)** is a **distributed file system** built on top of cloud storage (AWS S3, Azure Data Lake Storage, GCP GCS).  
It provides a **mount point** so you can interact with your cloud data just like a local file system.  

👉 Think of DBFS as a **bridge between Spark and cloud storage**.  

---

## 🧩 Key Features  
- 📦 **Storage Abstraction** → Access cloud storage like local files.  
- ⚡ **Optimized for Spark** → Fast read/write of big data.  
- 🔒 **Secure Access** → Controlled by Unity Catalog / IAM.  
- 🔄 **Persistent Storage** → Files remain even after cluster shutdown.  
- 🛠️ **Mounting** → Mount external cloud storage to `/mnt/`.  

---

## 📁 DBFS Structure  

| Path                          | Description |
|-------------------------------|-------------|
| `/FileStore/`                 | Public files, images, ML models, notebooks export. |
| `/databricks-datasets/`       | Sample datasets provided by Databricks. |
| `/mnt/`                       | External storage mounts (S3, ADLS, GCS). |
| `/tmp/`                       | Temporary storage (cluster-specific). |
| `/user/hive/warehouse/`       | Hive tables & Delta tables storage. |

---

## 🖼️ DBFS Design  

```

               ☁️ Cloud Storage (S3 / ADLS / GCS)
                            │
                            ▼
                  🔗 DBFS (Abstraction Layer)
                            │

┌──────────────┬──────────────┬──────────────┬──────────────┐
│ /FileStore   │ /databricks- │ /mnt         │ /tmp         │
│              │ datasets     │ (mounts)     │ (scratch)    │
└──────────────┴──────────────┴──────────────┴──────────────┘
│
▼
⚡ Access via Spark / Python / SQL

````

---

## 🧑‍💻 Accessing DBFS  

### 1️⃣ Using Databricks Utilities (`dbutils`)
```python
# List files
dbutils.fs.ls("/FileStore/")

# Copy file
dbutils.fs.cp("dbfs:/FileStore/data.csv", "dbfs:/mnt/raw/data.csv")

# Remove file
dbutils.fs.rm("dbfs:/tmp/sample.json")
````

---

### 2️⃣ Using `%fs` Magic Command

```bash
%fs ls /databricks-datasets/
%fs head /FileStore/data.csv
```

---

### 3️⃣ Using Spark APIs

```python
# Read CSV from DBFS
df = spark.read.csv("dbfs:/FileStore/data.csv", header=True, inferSchema=True)

# Write DataFrame to DBFS
df.write.format("delta").save("dbfs:/mnt/processed/sales_data")
```

---

### 4️⃣ Mounting External Storage

```python
dbutils.fs.mount(
  source = "wasbs://container@storageaccount.blob.core.windows.net/",
  mount_point = "/mnt/raw",
  extra_configs = {"fs.azure.account.key.storageaccount.blob.core.windows.net":"<access-key>"}
)
```

---

## ✅ Benefits of DBFS

* 📂 **Unified Access** → Same interface across AWS, Azure, GCP.
* ⚡ **High Performance** → Optimized for large-scale data workloads.
* 🔗 **Seamless Integration** → Works with Spark, MLflow, Delta Lake.
* 🔒 **Secure** → Supports encryption and fine-grained access control.
* 🛠️ **Developer Friendly** → Access via Python, R, Scala, SQL, CLI.

---

## 🌟 Example Use Case

1. Upload raw CSV data to `/FileStore/`.
2. Run Spark ETL jobs on `/mnt/raw/`.
3. Save results in `/mnt/processed/` as Delta tables.
4. Train ML models using data stored in DBFS.
5. Share files via `/FileStore/` for visualization.

---
