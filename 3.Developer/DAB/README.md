# 🚀 Databricks Asset Bundles (DAB)

---

## 🧠 What are Databricks Asset Bundles?

**Databricks Asset Bundles (DAB)** are a **modern way to package, deploy, and manage Databricks resources** using code 📦

👉 Think of it like:
> 🔧 Infrastructure as Code (IaC) for Databricks

---

## 🔥 Key Idea

> 💡 Define everything in code → Deploy anywhere (Dev / QA / Prod)

- ✅ Jobs
- ✅ Notebooks
- ✅ Pipelines (DLT / Lakeflow)
- ✅ Cluster configs

---

## 🧩 Core Components

### 1️⃣ `bundle.yml` (Main Config File)

Defines:
- Environments 🌍
- Resources ⚙️
- Deployment settings

```yaml
bundle:
  name: my_bundle

targets:
  dev:
    workspace:
      host: https://adb-xxxx.azuredatabricks.net

resources:
  jobs:
    my_job:
      name: sample-job
````

---

### 2️⃣ Resources

You can define:

#### ⚙️ Jobs

```yaml id="t3zvnt"
resources:
  jobs:
    etl_job:
      name: etl-job
      tasks:
        - task_key: task1
          notebook_task:
            notebook_path: ./notebooks/etl.py
```

---

#### 🔄 Pipelines (DLT / Lakeflow)

```yaml id="p7z5a9"
resources:
  pipelines:
    my_pipeline:
      name: dlt-pipeline
      libraries:
        - notebook:
            path: ./pipelines/dlt_pipeline.py
```

---

#### 🧠 Clusters

```yaml id="d43h2o"
resources:
  clusters:
    my_cluster:
      spark_version: 13.3.x-scala2.12
      node_type_id: Standard_DS3_v2
```

---

## ⚙️ 3. Environments (Targets)

```yaml id="9g6l3c"
targets:
  dev:
    workspace:
      host: https://dev-workspace

  prod:
    workspace:
      host: https://prod-workspace
```

### 🚀 Benefit:

* Deploy same code across environments

---

## 🔄 Deployment Commands

```bash
databricks bundle validate
databricks bundle deploy
databricks bundle run
```

---

## 📂 Project Structure

```text
my_bundle/
│
├── bundle.yml
├── notebooks/
│   └── etl.py
├── pipelines/
│   └── dlt_pipeline.py
```

---

## 🔥 Key Features

* 📦 Version-controlled deployments (Git)
* 🔁 CI/CD integration
* 🌍 Multi-environment support
* ⚙️ Reusable configurations
* 🔐 Secure deployments

---

## 🆚 Traditional vs Asset Bundles

| Feature         | Traditional ❌ | Asset Bundles ✅  |
| --------------- | ------------- | ---------------- |
| Deployment      | Manual UI     | Code-driven      |
| Version Control | Limited       | Full Git support |
| Reusability     | Low           | High             |
| CI/CD           | Hard          | Easy             |

---

## 🎯 Real-World Use Case

👉 Example: ETL Pipeline Deployment

* 🧑‍💻 Dev writes notebook
* 📦 Bundle defines job + pipeline
* 🔄 CI/CD deploys to Dev → QA → Prod
* ⚡ Same config reused everywhere

---

## ⚠️ Best Practices

* ✅ Use separate targets (dev/prod)
* ✅ Keep secrets outside config
* ✅ Modularize resources
* ✅ Use Git for version control
* ✅ Validate before deploy

---

## 🚫 Common Mistakes

* ❌ Hardcoding workspace URLs
* ❌ Mixing environments
* ❌ No version control
* ❌ Overcomplicated bundle.yml

---

## 🏁 Final Takeaway

👉 Databricks Asset Bundles =

* 📦 Package everything
* 🚀 Deploy consistently
* 🔄 Automate workflows

➡️ Essential for **production-grade Databricks projects**

---

## 🚀 Want More?

* 🔬 End-to-end CI/CD pipeline using DAB
* ⚙️ Real project structure for enterprise
* 🔐 Secrets & environment management

