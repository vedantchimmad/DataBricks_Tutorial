# Databricks Workflow Components

Databricks **Workflows** provide a way to orchestrate data and AI pipelines by chaining together tasks like notebooks, JARs, Python scripts, SQL queries, or external jobs.  
They replace the need for external schedulers by providing **end-to-end orchestration inside Databricks**.

---

## 🔹 Key Components of a Workflow

### 1. **Job**
- A workflow is created as a **Job** in Databricks.
- A job can contain **one or more tasks**.
- Each job has its own **schedule**, **cluster configuration**, and **notifications**.

---

### 2. **Task**
- The **building block** of a workflow.
- Each task performs one unit of work (e.g., running a notebook, executing SQL, or calling a Python file).
- Tasks can have **dependencies** → allowing sequential or parallel execution.

**Types of tasks:**
- **Notebook Task** → runs a Databricks notebook.
- **JAR Task** → executes a JAR file on the cluster.
- **Python Task** → runs a Python script.
- **SQL Task** → runs a SQL query or dashboard refresh.
- **dbt Task** → runs dbt commands for transformation.
- **Pipeline Task** → triggers a Delta Live Table (DLT) pipeline.
- **External Task** → calls an external job or API.
- **Custom Task** → shell commands or scripts.

---

### 3. **Clusters**
- Each workflow runs on a **Databricks cluster**.
- Options:
  - **Job Cluster** → temporary cluster created for the job and terminated after completion (cost-efficient).
  - **All-purpose Cluster** → existing shared cluster, useful for development/testing.

---

### 4. **Schedules**
- Define **when and how often** a workflow runs.
- Options:
  - **Manual Trigger** → run on demand.
  - **Scheduled** → cron-like scheduling (e.g., every day at midnight).
  - **Continuous/Streaming** → for Autoloader or real-time jobs.

---

### 5. **Parameters**
- Jobs can be parameterized to make workflows **dynamic**.
- Example: pass table name, file path, or date into a notebook task.

```python
dbutils.widgets.text("input_path", "/mnt/data/input/")
input_path = dbutils.widgets.get("input_path")
````

---

### 6. **Dependencies**

* Define **execution order** between tasks.
* Support:

    * **Sequential execution** (Task B waits for Task A).
    * **Parallel execution** (Tasks run at the same time if no dependency).
    * **Conditional execution** (run only if a parent task succeeds or fails).

---

### 7. **Triggers**

* Workflows can be triggered by:

    * **Schedule** (time-based).
    * **File arrival** (using Autoloader or external triggers).
    * **API call** (via Jobs REST API).
    * **Manual Run**.

---

### 8. **Notifications & Monitoring**

* Jobs can send **alerts** via:

    * Email
    * Slack / MS Teams
    * PagerDuty / Webhooks
* Monitoring UI shows:

    * **Run history**
    * **Success/Failure status**
    * **Logs & metrics**

---

### 9. **Version Control Integration**

* Tasks (especially notebooks) can be linked with **Git repos** for versioning.
* Supports GitHub, Azure DevOps, Bitbucket, GitLab.

---

## 🔹 Example: Workflow Structure

```text
Job: "Daily ETL Pipeline"
|
├── Task 1: Ingest Raw Data (Notebook, Job Cluster)
│
├── Task 2: Transform Data (Python Script, depends on Task 1)
│
├── Task 3: Load into Delta Lake (SQL Task, depends on Task 2)
│
└── Task 4: Refresh Dashboard (SQL Dashboard Task, parallel after Task 3)
```

---

## 🔹 Summary

| Component    | Purpose                                    |
| ------------ | ------------------------------------------ |
| Job          | Container for tasks, schedule, and configs |
| Task         | Unit of execution (Notebook, SQL, Python)  |
| Cluster      | Compute environment (Job / All-purpose)    |
| Schedule     | Defines when workflow runs                 |
| Parameters   | Dynamic input for workflows                |
| Dependencies | Task ordering and parallelization          |
| Triggers     | Schedule, file arrival, API, manual        |
| Monitoring   | Logs, run history, notifications           |

---

✅ In short:
A **Databricks Workflow** = **Job** + **Tasks** + **Dependencies** + **Cluster** + **Schedule/Trigger** + **Monitoring**.
