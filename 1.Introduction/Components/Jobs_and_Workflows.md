# ⏰ Jobs & 🔄 Workflows in Databricks  

---

## 🔹 What are Jobs?  
A **Job** in Databricks is a way to **automate a task** (e.g., ETL pipeline, ML model training, data ingestion, or reporting) so it can run on a **schedule or trigger** without manual intervention.  

👉 Jobs run **notebooks, JARs, Python scripts, or SQL queries** on clusters.  

---

## 🧩 Key Features of Jobs  
- 🛠️ **Task Execution** → Run notebooks, scripts, or SQL.  
- ⚡ **Job Clusters** → Temporary clusters created only for the job.  
- 📅 **Scheduling** → Daily, hourly, or custom cron schedule.  
- 🔔 **Alerts & Monitoring** → Notify on success/failure.  
- 🔒 **Secure Execution** → Uses Unity Catalog for permissions.  

---

## 🔹 What are Workflows?  
A **Workflow** in Databricks is a **collection of tasks** organized in a **directed acyclic graph (DAG)**.  
Each task can depend on others → allowing **ETL pipelines, ML pipelines, and complex workflows**.  

👉 Think of workflows as **Jobs + Task Orchestration**.  

---

## 🧩 Key Features of Workflows  
- 🔄 **Task Dependencies** → Define order (e.g., Task B after Task A).  
- 🚀 **Parallel Execution** → Run independent tasks simultaneously.  
- 🧰 **Multi-Task Jobs** → Combine notebooks, scripts, JARs, SQL.  
- 📊 **Visual DAG** → Drag-and-drop UI to see execution flow.  
- 🕒 **Retries & Timeout** → Control execution behavior.  
- 🔗 **Integration** → Trigger from API, CLI, or other tools (Airflow, Azure Data Factory).  

---

## 🖼️ Jobs vs Workflows  

| Feature                | Jobs ⏰ | Workflows 🔄 |
|-------------------------|---------|--------------|
| Runs single task        | ✅ Yes  | ❌ No         |
| Runs multiple tasks     | ❌ No   | ✅ Yes        |
| Supports DAG            | ❌ No   | ✅ Yes        |
| Scheduling              | ✅ Yes  | ✅ Yes        |
| Error handling & retry  | Basic   | Advanced      |
| Use case                | Simple automation | Complex pipelines |

---

## 🖼️ Databricks Workflow Design  

```
            🔄 Databricks Workflow (Pipeline)

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Task 1      │──▶──│   Task 2     │──▶──│   Task 3     │
│ (Ingest Data)│     │ (Transform)  │     │ (Load to DB) │
└──────────────┘     └──────────────┘     └──────────────┘
│
▼
📊 Dashboard / ML Model

````

---

## 🧑‍💻 Example: Creating a Job via Python API  

```python
from databricks_api import DatabricksAPI

# Connect to Databricks
db = DatabricksAPI(
    host="https://<your-databricks-instance>",
    token="<your-access-token>"
)

# Create Job
job_config = {
    "name": "etl_pipeline",
    "new_cluster": {
        "spark_version": "11.3.x-scala2.12",
        "node_type_id": "Standard_DS3_v2",
        "num_workers": 2
    },
    "notebook_task": {
        "notebook_path": "/Repos/ETL/etl_notebook"
    }
}

db.jobs.create_job(**job_config)
````

---

## ✅ Benefits of Jobs & Workflows

* ⏰ **Automation** → No need for manual runs.
* 🔄 **Scalable Pipelines** → Chain multiple tasks easily.
* 📊 **Visibility** → DAG visualization and monitoring.
* 🚀 **Productivity** → Faster, reusable pipelines for ETL & ML.
* 🔔 **Reliability** → Alerts, retries, and error handling.

---

## 🌟 Example Use Case

1. **Ingest** → Load raw data from S3/ADLS into Delta Lake.
2. **Transform** → Clean & aggregate data with Spark notebooks.
3. **Train ML Model** → Run MLflow training job.
4. **Publish** → Save results to Delta tables.
5. **Dashboard** → Refresh Power BI/Tableau dashboards.
6. **Schedule** → Run every day at 1 AM automatically.

---

