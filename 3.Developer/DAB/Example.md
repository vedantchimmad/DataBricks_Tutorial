# 🚀 Databricks Asset Bundle (DAB) for Dev, Test & Prod Environments

---

## 🧠 Goal

👉 Create a **single bundle** that can deploy to:
- 🧪 Dev (Development)
- 🔍 Test (QA/UAT)
- 🚀 Prod (Production)

---

# 📂 Project Structure

```text
my_bundle/
│
├── bundle.yml
├── notebooks/
│   └── etl.py
├── pipelines/
│   └── dlt_pipeline.py
````

---

# ⚙️ Complete `bundle.yml`

```yaml
bundle:
  name: etl_bundle

# 🌍 Environments
targets:
  dev:
    workspace:
      host: https://adb-dev.azuredatabricks.net
    default: true
    resources:
      jobs:
        etl_job:
          name: etl-job-dev

  test:
    workspace:
      host: https://adb-test.azuredatabricks.net
    resources:
      jobs:
        etl_job:
          name: etl-job-test

  prod:
    workspace:
      host: https://adb-prod.azuredatabricks.net
    resources:
      jobs:
        etl_job:
          name: etl-job-prod

# 📦 Resources (Common Definition)
resources:

  jobs:
    etl_job:
      name: etl-job
      tasks:
        - task_key: etl-task
          notebook_task:
            notebook_path: ./notebooks/etl.py
          existing_cluster_id: ${var.cluster_id}

  pipelines:
    dlt_pipeline:
      name: dlt-pipeline
      libraries:
        - notebook:
            path: ./pipelines/dlt_pipeline.py
      target: ${bundle.target}

# 🔐 Variables (Environment-specific values)
variables:
  cluster_id:
    description: Cluster ID for each environment

---

# 🎯 Environment-specific overrides
targets:

  dev:
    variables:
      cluster_id: "dev-cluster-id"

  test:
    variables:
      cluster_id: "test-cluster-id"

  prod:
    variables:
      cluster_id: "prod-cluster-id"
```

---

# 🔄 Deployment Commands

## 🧪 Deploy to Dev

```bash
databricks bundle deploy --target dev
```

## 🔍 Deploy to Test

```bash id="x1i5fg"
databricks bundle deploy --target test
```

## 🚀 Deploy to Prod

```bash id="n3a1lj"
databricks bundle deploy --target prod
```

---

# ▶️ Run Job

```bash
databricks bundle run etl_job --target dev
```

---

# 🔥 Key Features in This Setup

* 🌍 Multi-environment support
* 🔐 Environment-specific variables
* 🔄 Reusable resource definitions
* 📦 Single codebase for all deployments

---

# ⚠️ Best Practices

## ✅ Environment Isolation

* Use separate:

    * Workspaces
    * Clusters
    * Storage paths

---

## 🔐 Secrets Management

* Store secrets in:

    * Databricks Secret Scope
    * Azure Key Vault / AWS Secrets Manager

---

## 📦 CI/CD Integration

* Use:

    * GitHub Actions
    * Azure DevOps

---

## 🧠 Naming Convention

* dev → `etl-job-dev`
* test → `etl-job-test`
* prod → `etl-job-prod`

---

# 🚫 Common Mistakes

* ❌ Hardcoding cluster IDs
* ❌ Using same workspace for all env
* ❌ No variable separation
* ❌ Manual deployment

---

# 🎯 Pro Architecture

```text
Developer → Git Push → CI/CD → Deploy Bundle → Dev → Test → Prod
```

---

# 🏁 Final Takeaway

👉 With DAB:

* 📦 One bundle → Multiple environments
* 🔄 Easy deployment
* 🚀 Production-ready pipelines

---
