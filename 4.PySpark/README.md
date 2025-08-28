# ⚡ Apache Spark on Databricks

---

## 🔹 What is Spark on Databricks?
Apache Spark is an **open-source distributed computing framework** for processing big data.  
**Databricks** provides a **managed Spark environment** with auto-scaling clusters, integrations, and optimizations, making Spark **faster, easier, and more reliable**.

👉 In short: **Databricks = Spark + Cloud + Collaboration + Optimizations**

---

## 🏗️ Spark Architecture on Databricks


```
              👥 Users (SQL, Python, Scala, R)
                           │
                           ▼
                  🖥️ Databricks Workspace
        ────────────────────────────────────────
        | 📒 Notebooks | ⏰ Jobs | 📊 Dashboards |
        ────────────────────────────────────────
                           │
                           ▼
                  ⚡ Spark Clusters (Compute)
        ────────────────────────────────────────
        | Driver Node | Executor Nodes (Workers) |
        ────────────────────────────────────────
                           │
                           ▼
               ☁️ Cloud Storage (Delta Lake, S3, ADLS, GCS)
```

---

## 🔑 Why Spark on Databricks?
- 🚀 **Performance** – Optimized Spark runtime (faster than open-source Spark).  
- 🔄 **Elastic Scaling** – Auto-scaling clusters handle variable workloads.  
- 🛠️ **Ease of Use** – No manual cluster setup; managed by Databricks.  
- 📂 **Integration** – Works seamlessly with Delta Lake, MLflow, Unity Catalog.  
- 👨‍💻 **Multi-language Support** – Python (PySpark), Scala, R, SQL.  
- 📊 **Unified Workloads** – ETL, ML, streaming, and analytics in one platform.  

---

## 🧩 Spark Components on Databricks
| Component        | Role in Databricks |
|------------------|--------------------|
| **Driver Node**  | Runs main program, schedules tasks |
| **Executor Nodes** | Run tasks, store cached data |
| **Cluster Manager** | Databricks manages Spark clusters automatically |
| **Storage Layer** | Integrated with cloud (S3, ADLS, GCS) + Delta Lake |
| **Databricks Runtime** | Optimized Spark distribution with libraries & connectors |

---

## 🔍 Example: PySpark on Databricks
### ✅ Creating DataFrame
```python
# Create DataFrame from Python data
data = [("Alice", 25), ("Bob", 30), ("Cathy", 28)]
columns = ["Name", "Age"]

df = spark.createDataFrame(data, columns)
df.show()
````

### ✅ Reading from Cloud Storage

```python
# Read from Delta table
df = spark.read.format("delta").load("/mnt/data/customers")

# Read from CSV
csv_df = spark.read.csv("/mnt/raw/customers.csv", header=True, inferSchema=True)
```

### ✅ Transformations

```python
from pyspark.sql.functions import col

df_transformed = df.withColumn("Age_plus_5", col("Age") + 5)
df_transformed.show()
```

### ✅ Writing Data

```python
# Write to Delta table
df_transformed.write.format("delta").mode("overwrite").save("/mnt/data/customers_delta")
```

---

## 🔄 Spark Workloads on Databricks

* 📥 **Batch Processing** → ETL jobs with Spark SQL, DataFrames.
* 🔄 **Streaming Processing** → Real-time analytics with Structured Streaming.
* 🤖 **Machine Learning** → MLlib + MLflow integration.
* 📊 **Analytics & BI** → SQL queries, dashboards.

---

## ⚡ Advanced Spark on Databricks

* **Delta Lake** → ACID transactions & time travel.
* **Photon Engine** → Next-gen query engine for faster execution.
* **Z-Ordering & Optimize** → Query performance improvements.
* **Adaptive Query Execution (AQE)** → Auto-optimizes joins & shuffles.
* **Unity Catalog** → Centralized data governance.

---

## 🏆 Benefits of Spark on Databricks

* ✅ No cluster management headache.
* ✅ Best-in-class performance with Databricks runtime.
* ✅ Supports batch, streaming, ML, and BI in one platform.
* ✅ Scales to petabytes with cloud-native elasticity.
* ✅ Ideal for **modern Lakehouse architecture**.

---