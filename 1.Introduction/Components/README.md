# ⚙️ Databricks Components  

---

## 🔹 Overview  
Databricks is built with several **core components** that work together to provide a **unified data and AI platform**.  
These components help in **data engineering, machine learning, analytics, governance, and collaboration**.  

---

## 🧩 Core Components of Databricks  

### 1️⃣ Workspace  
- 🖥️ **What it is** → A collaborative environment for teams.  
- 📒 **Includes**:  
  - **Notebooks** (multi-language: Python, SQL, Scala, R)  
  - **Dashboards** for visualization  
  - **Repos** for Git integration  
- 🤝 Enables real-time collaboration among engineers, scientists, and analysts.  

---

### 2️⃣ Clusters  
- ⚡ **What it is** → The compute layer where Spark jobs run.  
- 🔄 **Types**:  
  - **Interactive Cluster** → For development and notebooks  
  - **Job Cluster** → Auto-created per job, terminated after completion  
  - **High Concurrency Cluster** → For multiple users simultaneously  
- 🛠️ **Features**: Autoscaling, auto-termination, multiple Spark runtimes  

---

### 3️⃣ Jobs & Workflows  
- ⏰ **Jobs** → Automate running notebooks, scripts, or JARs.  
- 🔄 **Workflows** → Chain multiple tasks (ETL → ML → Reports).  
- 📅 **Triggers** → Run jobs on schedule or event-based execution.  

---

### 4️⃣ Libraries  
- 📦 External packages used in Databricks.  
- 📚 Sources include:  
  - PyPI (Python packages)  
  - Maven (Java/Scala packages)  
  - Uploaded JAR/Wheel files  
- 🔧 Attached to clusters for extending Spark functionality.  

---

### 5️⃣ Databricks File System (DBFS)  
- 📂 A distributed file system abstraction over cloud storage.  
- 🗂️ Paths:  
  - `/FileStore` → Public file sharing  
  - `/mnt` → Mounted cloud storage (S3, ADLS, GCS)  
- 🔄 Fully integrated with Delta Lake.  

---

### 6️⃣ Delta Lake  
- 🏞️ Storage layer that brings **ACID transactions** to big data.  
- 🔑 Features:  
  - ✅ Schema enforcement & evolution  
  - ⏳ Time Travel (query old versions)  
  - 🔄 Merge (Upserts)  
  - ⚡ Optimized queries with Z-ordering  

---

### 7️⃣ Unity Catalog  
- 🔒 Centralized **data governance & security layer**.  
- 🧩 Features:  
  - Manage users, groups, and permissions  
  - Fine-grained access (table/column/row level)  
  - Cross-workspace governance  

---

### 8️⃣ Databricks SQL  
- 📊 SQL-first interface for querying data.  
- 🔧 Features:  
  - Native SQL editor in workspace  
  - BI dashboards & alerts  
  - Direct access to Delta tables  

---

### 9️⃣ MLflow Integration  
- 🤖 Open-source ML lifecycle management integrated into Databricks.  
- 📈 Features:  
  - Experiment tracking  
  - Model versioning  
  - Model registry & deployment  

---

### 🔟 Databricks Utilities (`dbutils`)  
- 🛠️ Helper commands for interacting with Databricks.  
- ⚡ Categories:  
  - `dbutils.fs` → File system operations  
  - `dbutils.secrets` → Manage secrets & credentials  
  - `dbutils.widgets` → Parameterize notebooks  

---

## 🖼️ Databricks Components Design  

```
            👥 Users & Teams

──────────────────────────────────────────
| Data Engineers | Scientists | Analysts |
──────────────────────────────────────────
│
▼
🖥️ Databricks Workspace (Notebooks, Dashboards, Repos)
│
┌───────────┴───────────┐
▼                       ▼
⚡ Clusters (Compute)     📂 DBFS / Delta Lake (Storage)
│                       │
└───────────────┬───────┘
▼
🔒 Unity Catalog (Governance & Security)
│
▼
📊 Databricks SQL + 🤖 MLflow (Analytics & AI)

```

---

## ✅ Summary  
Databricks Components work together to provide:  
- **Data Engineering** → Clusters, DBFS, Delta Lake  
- **Data Science** → MLflow, Notebooks  
- **Analytics** → SQL, Dashboards  
- **Governance** → Unity Catalog  
- **Collaboration** → Workspace, Repos  

---
