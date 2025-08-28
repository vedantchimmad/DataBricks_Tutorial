# ⚙️ Different Ways to Create Compute in Databricks  

---

## 🔹 Introduction  
Databricks provides multiple ways to **create compute resources** (clusters, SQL warehouses, job clusters, serverless compute, ML compute).  
Depending on your **persona** (Data Engineer, Data Scientist, Analyst, ML Engineer), you can create compute via **UI, API, CLI, or Infrastructure-as-Code (IaC)**.  

---

## 🧩 Ways to Create Compute  

### 1️⃣ **Databricks UI (Workspace)** 🖥️  
- Easiest & most common method.  
- Interactive web interface in the **Databricks Workspace**.  

✅ Steps:  
1. Go to **Compute** in the left navigation bar.  
2. Select **Create Compute** (Cluster / SQL Warehouse / Job Cluster).  
3. Configure settings (runtime, workers, autoscaling, libraries).  
4. Launch the cluster or warehouse.  

🔍 Example: Creating an **interactive cluster** to run PySpark notebooks.  

---

### 2️⃣ **Databricks CLI** 💻  
- Command-line tool to automate compute creation.  

✅ Example: Create a cluster with a JSON config file.  

```bash
databricks clusters create --json '{
  "cluster_name": "demo-cluster",
  "spark_version": "11.3.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 2
}'
````

🔍 Best for: DevOps engineers automating environments.

---

### 3️⃣ **Databricks REST API** 🌐

* Direct API calls to programmatically create and manage compute.
* Flexible and integration-friendly (CI/CD pipelines).

✅ Example: Create a job cluster via REST API.

```http
POST /api/2.0/clusters/create
Content-Type: application/json

{
  "cluster_name": "etl-job-cluster",
  "spark_version": "11.3.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 2
}
```

🔍 Best for: Automation with scripts or CI/CD pipelines.

---

### 4️⃣ **Jobs UI / Workflows** 🏭

* When creating a **Job** in Databricks, you can configure a **Job Cluster**.
* The cluster will **start when the job runs** and **terminate automatically** after.

🔍 Best for: Production ETL, scheduled data pipelines.

---

### 5️⃣ **Databricks SQL UI** 📊

* In **Databricks SQL** workspace, you can create:

    * **Classic SQL Warehouses** (dedicated clusters).
    * **Serverless SQL Warehouses** (Databricks-managed).

🔍 Best for: BI Analysts running queries and dashboards.

---

### 6️⃣ **Infrastructure as Code (IaC)** 🏗️

* Use **Terraform, Azure ARM templates, AWS CloudFormation, or Databricks Terraform Provider**.
* Define compute resources as **code** for reproducibility.

✅ Example (Terraform):

```hcl
resource "databricks_cluster" "this" {
  cluster_name  = "terraform-cluster"
  spark_version = "11.3.x-scala2.12"
  node_type_id  = "Standard_DS3_v2"
  num_workers   = 2
}
```

🔍 Best for: Enterprise environments with strict governance.

---

## 🖼️ Visual Summary

```
     🖥️ UI (Workspace) → For Data Engineers & Scientists (interactive)
     💻 CLI → For DevOps automation
     🌐 REST API → For CI/CD pipelines
     🏭 Jobs UI → For ETL pipelines & workflows
     📊 SQL UI → For BI dashboards & analysts
     🏗️ IaC (Terraform/ARM/CloudFormation) → For enterprise governance
```

---

## ✅ Recommendation

| Persona         | Preferred Way to Create Compute    |
| --------------- | ---------------------------------- |
| Data Engineer   | 🖥️ Workspace UI, 🏭 Job Clusters  |
| Data Scientist  | 🖥️ Interactive Clusters, 💻 CLI   |
| BI Analyst      | 📊 SQL Warehouses (UI/Serverless)  |
| ML Engineer     | 🤖 ML Clusters (UI + ML Runtime)   |
| DevOps Engineer | 🌐 REST API, 💻 CLI, 🏗️ Terraform |

---

🚀 With these options, you can create **compute in Databricks** flexibly — from quick exploration to enterprise-scale production pipelines.

---
# ⚡ Special Parameters for Creating Different Types of Clusters using Databricks CLI

When creating clusters with the **Databricks CLI**, the JSON payload you pass to  
`databricks clusters create --json-file <file>.json`  
determines the **type of cluster** you get.  

Below are the **special parameters** needed for each cluster type:

---

## 1️⃣ Interactive Cluster (for development & notebooks)
Interactive clusters are used for **ad-hoc exploration** and **notebook execution**.

```json
{
  "cluster_name": "dev-interactive-cluster",
  "spark_version": "12.2.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 2,
  "autotermination_minutes": 30,   // ⏳ auto-shutdown (optional)
  "cluster_source": "UI"           // ✅ Special parameter for interactive cluster
}
````

**🔑 Special parameter:**

* `"cluster_source": "UI"` → Marks the cluster as interactive (user-facing).

---

## 2️⃣ Job (Automated) Cluster

Job clusters are **ephemeral** (created only for job runs and terminated afterward).

```json
{
  "cluster_name": "etl-job-cluster",
  "spark_version": "12.2.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 2,
  "cluster_source": "JOB"          // ✅ Special parameter for job cluster
}
```

**🔑 Special parameter:**

* `"cluster_source": "JOB"` → Marks the cluster as job-only.

---

## 3️⃣ High-Concurrency Cluster

Designed for **multiple users / BI tools** (Databricks SQL, JDBC/ODBC).

```json
{
  "cluster_name": "bi-high-concurrency-cluster",
  "spark_version": "12.2.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 4,
  "spark_conf": {
    "spark.databricks.cluster.profile": "serverless"   // ✅ High concurrency config
  },
  "custom_tags": {
    "usage": "BI"
  }
}
```

**🔑 Special parameter:**

* `"spark_conf": { "spark.databricks.cluster.profile": "serverless" }`
  → Configures cluster for **high concurrency**.

---

## 4️⃣ Serverless SQL Warehouse (via CLI)

Not exactly a cluster, but managed compute for SQL.
Created using **`databricks sql warehouses create`** instead of `clusters create`.

```json
{
  "name": "serverless-sql-wh",
  "cluster_size": "Small",
  "max_num_clusters": 1,
  "enable_serverless_compute": true   // ✅ Special parameter for serverless SQL
}
```

**🔑 Special parameter:**

* `"enable_serverless_compute": true` → Enables **serverless SQL Warehouse**.

---

## 5️⃣ Single-Node Cluster

Useful for ML model training, lightweight tasks, or local-like execution.

```json
{
  "cluster_name": "ml-single-node-cluster",
  "spark_version": "12.2.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "num_workers": 0,                          // ✅ Must be 0
  "spark_conf": {
    "spark.master": "local[*]"               // ✅ Local mode
  }
}
```

**🔑 Special parameters:**

* `"num_workers": 0` → Forces single-node mode.
* `"spark_conf": { "spark.master": "local[*]" }`

---

## 📊 Summary Table

| Cluster Type      | Special Parameter(s)                                                 |
| ----------------- | -------------------------------------------------------------------- |
| Interactive       | `"cluster_source": "UI"`                                             |
| Job               | `"cluster_source": "JOB"`                                            |
| High-Concurrency  | `"spark_conf": { "spark.databricks.cluster.profile": "serverless" }` |
| Serverless SQL WH | `"enable_serverless_compute": true` (via SQL API, not clusters API)  |
| Single-Node       | `"num_workers": 0` + `"spark_conf": { "spark.master": "local[*]" }`  |

---

✅ With these parameters, you can control exactly which **type of cluster** gets created when using the **Databricks CLI**.

---
