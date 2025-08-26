# ⚡ Databricks Clusters  

---

## 🔹 What is a Cluster in Databricks?  
A **cluster** in Databricks is a **set of virtual machines (nodes)** that run **Apache Spark workloads**.  
Clusters provide the **compute power** needed to process data, train ML models, and run analytics in Databricks.  

👉 Simply put: A **cluster = compute engine** for all your data tasks in Databricks.  

---

## 🧩 Cluster Components  

### 1️⃣ Driver Node  
- 🧠 Acts as the **master node**.  
- 📋 Runs the main Spark program.  
- 🗂️ Distributes tasks to executor nodes.  
- 🔄 Collects results from executors.  

### 2️⃣ Worker/Executor Nodes  
- 💻 Perform actual **data processing** tasks.  
- 📦 Store cached data.  
- ⚡ Run transformations & actions on partitions of data.  

---

## 🔄 Types of Clusters in Databricks  

| Cluster Type            | Usage                                                    |
|-------------------------|----------------------------------------------------------|
| **Interactive Cluster** | 👨‍💻 Development, ad-hoc analysis, notebooks            |
| **Job Cluster**         | ⏰ Automated workloads (created per job, auto-terminated) |
| **High-Concurrency**    | 📊 For BI tools, serving multiple users simultaneously   |

---

## ⚙️ Cluster Configuration Options  
When creating a cluster in Databricks, you can configure:  
- **Databricks Runtime** → Spark version with optimized libraries.  
- **Node Type** → VM type (CPU, memory, storage).  
- **Autoscaling** → Add/remove workers based on workload.  
- **Auto Termination** → Shut down idle clusters to save cost.  
- **Libraries** → Attach Python, R, Java/Scala packages.  

---

## 🖼️ Databricks Cluster Architecture  

```
                ⚡ Databricks Cluster

──────────────────────────────────────────
| 🧠 Driver Node → Schedules & coordinates |
──────────────────────────────────────────
| 💻 Executor Node 1 → Runs tasks          |
| 💻 Executor Node 2 → Runs tasks          |
| 💻 Executor Node N → Runs tasks          |
──────────────────────────────────────────
│
▼
☁️ Cloud Storage (S3, ADLS, GCS, Delta Lake)

````

---

## 🧑‍💻 Example: Creating a Cluster via UI  
1. Go to **Clusters** in Databricks Workspace.  
2. Click **Create Cluster**.  
3. Configure:  
   - Cluster Name: `etl-cluster`  
   - Runtime: `11.3.x-scala2.12 (Databricks Runtime)`  
   - Node Type: `Standard_DS3_v2`  
   - Workers: `2-8` (autoscaling enabled)  
4. Create and attach a notebook to the cluster.  

---

## 🔧 Example: Using Spark in a Cluster  
```python
# Create DataFrame
data = [("Alice", 25), ("Bob", 30)]
columns = ["Name", "Age"]

df = spark.createDataFrame(data, columns)

# Transformation
from pyspark.sql.functions import col
df2 = df.withColumn("Age_plus_5", col("Age") + 5)

# Show results
df2.show()
````

---

## ✅ Benefits of Databricks Clusters

* 🚀 **Elastic Scaling** – Add/remove nodes automatically.
* ⏱️ **On-Demand Compute** – Pay only for what you use.
* 🔧 **Optimized Spark Runtime** – Faster than open-source Spark.
* 🔄 **Reusable** – Interactive clusters for development, job clusters for ETL.
* 🔒 **Secure** – Fine-grained access control via Unity Catalog.

---
