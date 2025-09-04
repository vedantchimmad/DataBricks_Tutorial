# Databricks Asset Bundles (DABs)

**Databricks Asset Bundles (DABs)** are a way to define, organize, and deploy **Databricks resources** (jobs, workflows, pipelines, dashboards, etc.) as **code**.  
They bring **DevOps practices (CI/CD, version control, reproducibility)** into Databricks projects.

---

## 🔹 What is a Databricks Asset Bundle?

- A **bundle** is a collection of configuration files that describe Databricks assets.
- Assets are defined in **YAML/JSON files**, which can be stored in a Git repo.
- Bundles allow you to:
  - Define multiple assets together (jobs, pipelines, models, dashboards).
  - Deploy them consistently across **environments** (dev, test, prod).
  - Integrate with **CI/CD pipelines**.

---

## 🔹 Key Components of a Bundle

1. **`databricks.yml`**  
   - Main configuration file for the bundle.
   - Defines:
     - Jobs (tasks, clusters, dependencies).
     - Pipelines (DLT).
     - SQL assets (dashboards, queries).
     - Deployment environments (dev, staging, prod).

   Example:
```yaml
   bundle:
     name: sales-etl-pipeline
   jobs:
     etl-job:
       tasks:
         - task_key: ingest
           notebook_task:
             notebook_path: ./notebooks/ingest
           existing_cluster_id: "cluster-123"
         - task_key: transform
           depends_on: [ingest]
           python_wheel_task:
             package_name: etl
             entry_point: main
````

---

2. **Jobs**

    * Define **workflows** (notebooks, Python scripts, SQL tasks).
    * Includes cluster configuration, schedules, and dependencies.

3. **Pipelines**

    * Define **Delta Live Table (DLT) pipelines** for ETL.
    * Example:

      ```yaml
      pipelines:
        daily-sales-pipeline:
          target: sales_gold
          libraries:
            - notebook: ./dlt/sales_transform
      ```

4. **SQL Assets**

    * Dashboards, alerts, and queries can also be bundled.

5. **Environments**

    * Bundles can be deployed across multiple **environments**.
    * Example:

      ```yaml
      targets:
        dev:
          workspace:
            host: https://adb-1234.0.azuredatabricks.net
        prod:
          workspace:
            host: https://adb-5678.0.azuredatabricks.net
      ```

---

## 🔹 Lifecycle of a Bundle

1. **Develop**

    * Write notebooks, jobs, pipelines, dashboards.
    * Define them in `databricks.yml`.

2. **Test Locally**

    * Use `databricks bundle validate` to check configuration.

3. **Deploy**

    * Deploy to a specific environment:

      ```bash
      databricks bundle deploy -t dev
      databricks bundle deploy -t prod
      ```

4. **Run**

    * Execute a job from the bundle:

      ```bash
      databricks bundle run etl-job -t dev
      ```

5. **CI/CD Integration**

    * Integrate with GitHub Actions, Azure DevOps, or Jenkins to automatically deploy bundles.

---

## 🔹 Benefits of Asset Bundles

* **Infrastructure as Code (IaC)** → reproducible, consistent deployments.
* **Multi-environment support** → dev/test/prod configs in one repo.
* **Version Control** → track changes via Git.
* **CI/CD Ready** → automation-friendly.
* **Standardization** → single format for all Databricks resources.

---

## 🔹 Example Project Structure

```text
sales-etl-bundle/
│
├── databricks.yml        # Main config
├── notebooks/            # ETL notebooks
│   ├── ingest.py
│   └── transform.py
├── dlt/                  # Delta Live Table pipelines
│   └── sales_transform.py
├── sql/                  # SQL dashboards & queries
│   └── sales_dashboard.sql
└── tests/                # Unit tests
```

---

## 🔹 Summary

| Feature          | Purpose                                  |
| ---------------- | ---------------------------------------- |
| `databricks.yml` | Defines jobs, pipelines, dashboards      |
| Jobs             | Workflow orchestration (tasks, clusters) |
| Pipelines (DLT)  | Streaming/batch ETL pipelines            |
| SQL Assets       | Dashboards, alerts, queries              |
| Environments     | Dev/test/prod deployment configs         |
| CLI (`bundle`)   | Validate, deploy, run workflows          |

---

✅ In short:
**Databricks Asset Bundles = “Deploy Databricks projects as code”** → enabling **versioning, automation, and multi-environment reproducibility**.
