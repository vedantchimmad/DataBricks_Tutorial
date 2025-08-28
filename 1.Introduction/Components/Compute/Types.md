# ⚙️ Types of Compute in Databricks  

---

## 🔹 Introduction  
In **Databricks**, **compute** refers to the processing resources used to run **data engineering, machine learning, and analytics workloads**.  
Databricks provides different **compute options** depending on your use case — from **interactive development** to **production-scale pipelines**.  

---

## 🧩 Types of Compute in Databricks  

### 1️⃣ **Interactive Clusters** 🖥️  
- **Purpose** → Used by data engineers, analysts, and scientists for **ad-hoc queries, development, and experimentation**.  
- **Characteristics**:  
  - Shared among multiple users.  
  - Can install custom libraries.  
  - Best for **notebooks** and **exploration**.  

🔍 Example Use: Running PySpark code in a notebook for data transformation.  

---

### 2️⃣ **Job Clusters** 🏭  
- **Purpose** → **Dedicated, ephemeral clusters** created for running jobs (ETL pipelines, scheduled workflows).  
- **Characteristics**:  
  - Created when a job starts, terminated when it finishes.  
  - Cost-efficient (no idle cluster).  
  - Best for **production pipelines**.  

🔍 Example Use: Running a daily batch ETL job to load data from Data Lake to Delta Lake.  

---

### 3️⃣ **SQL Warehouses (Databricks SQL)** 📊  
- **Purpose** → Compute for **SQL queries, dashboards, and BI tools**.  
- **Characteristics**:  
  - Powered by **Photon engine** for high performance.  
  - **Serverless** or **classic** options available.  
  - Scales automatically depending on query load.  
  - Integrated with **Unity Catalog** for governance.  

🔍 Example Use: Running dashboards in Power BI or Tableau connected to Databricks SQL.  

---

### 4️⃣ **Serverless Compute** ⚡  
- **Purpose** → Fully managed compute where **Databricks automatically provisions and scales resources**.  
- **Characteristics**:  
  - No cluster setup needed.  
  - Eliminates idle cost.  
  - Great for **ad-hoc queries and BI**.  

🔍 Example Use: Analyst runs an SQL query directly without worrying about cluster creation.  

---

### 5️⃣ **High-Concurrency Clusters** 👥  
- **Purpose** → Designed for **serving concurrent queries** from multiple users or BI tools.  
- **Characteristics**:  
  - Optimized for low-latency workloads.  
  - Uses **query isolation** to protect users from each other.  
  - Ideal for **BI dashboards** with many users.  

🔍 Example Use: Running 200 concurrent SQL queries for enterprise BI reporting.  

---

### 6️⃣ **Compute for Machine Learning** 🤖  
- **Purpose** → Specialized clusters for **ML model training and inference**.  
- **Characteristics**:  
  - Support for **GPU and CPU instances**.  
  - Integrated with **MLflow** for tracking and deployment.  
  - Often used with **Databricks Runtime for ML**.  

🔍 Example Use: Training deep learning models on GPU-accelerated clusters.  

---

## 🖼️ Visual Overview  

```
                  ⚙️ Databricks Compute Options

┌─────────────────────────┬─────────────────────────┬───────────────────────────┐
│  🖥️ Interactive Clusters │  🏭 Job Clusters        │  📊 SQL Warehouses         │
│  For dev & exploration  │  For scheduled jobs     │  For BI & SQL dashboards  │
└─────────────────────────┴─────────────────────────┴───────────────────────────┘
┌─────────────────────────┬─────────────────────────┬───────────────────────────┐
│ ⚡ Serverless Compute    │ 👥 High-Concurrency     │ 🤖 ML Compute (GPU/CPU)   │
│ Auto-scaling & managed  │ Concurrent BI queries   │ Model training & serving  │
└─────────────────────────┴─────────────────────────┴───────────────────────────┘

```

---

## ✅ Choosing the Right Compute  

| Use Case                         | Best Compute Type         |
|----------------------------------|---------------------------|
| Notebook development             | 🖥️ Interactive Cluster     |
| ETL pipelines & batch jobs       | 🏭 Job Cluster             |
| BI dashboards (Power BI/Tableau) | 📊 SQL Warehouse / 👥 High Concurrency |
| Ad-hoc SQL queries               | ⚡ Serverless Compute      |
| Machine Learning training        | 🤖 ML Compute (GPU/CPU)   |

---

🚀 **Databricks flexibility** → You can mix compute types in the same workspace depending on the team’s workload.  

---