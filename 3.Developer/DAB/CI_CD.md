# 🚀 CI/CD Pipeline YAML for Databricks Asset Bundles (DAB)

---

# 🧠 Goal

Automate deployment of **Databricks Asset Bundles (DAB)** across:
- 🧪 Dev
- 🔍 Test
- 🚀 Prod

---

# 🟦 Azure DevOps Pipeline (azure-pipelines.yml)

## 📦 Features
- Install Databricks CLI
- Validate bundle
- Deploy to environments
- Manual approval for Prod

---

## ⚙️ YAML

```yaml
trigger:
  branches:
    include:
      - main

variables:
  DATABRICKS_HOST_DEV: "https://adb-dev.azuredatabricks.net"
  DATABRICKS_HOST_TEST: "https://adb-test.azuredatabricks.net"
  DATABRICKS_HOST_PROD: "https://adb-prod.azuredatabricks.net"

stages:

# 🧪 DEV DEPLOYMENT
- stage: Deploy_Dev
  jobs:
    - job: Dev
      steps:
        - task: UsePythonVersion@0
          inputs:
            versionSpec: '3.x'

        - script: |
            pip install databricks-cli
          displayName: Install Databricks CLI

        - script: |
            databricks bundle validate
          displayName: Validate Bundle

        - script: |
            databricks bundle deploy --target dev
          env:
            DATABRICKS_HOST: $(DATABRICKS_HOST_DEV)
            DATABRICKS_TOKEN: $(DATABRICKS_TOKEN_DEV)
          displayName: Deploy to Dev

---

# 🔍 TEST DEPLOYMENT
- stage: Deploy_Test
  dependsOn: Deploy_Dev
  jobs:
    - job: Test
      steps:
        - script: |
            databricks bundle deploy --target test
          env:
            DATABRICKS_HOST: $(DATABRICKS_HOST_TEST)
            DATABRICKS_TOKEN: $(DATABRICKS_TOKEN_TEST)
          displayName: Deploy to Test

---

# 🚀 PROD DEPLOYMENT (Manual Approval)
- stage: Deploy_Prod
  dependsOn: Deploy_Test
  condition: succeeded()
  jobs:
    - job: Prod
      steps:
        - script: |
            databricks bundle deploy --target prod
          env:
            DATABRICKS_HOST: $(DATABRICKS_HOST_PROD)
            DATABRICKS_TOKEN: $(DATABRICKS_TOKEN_PROD)
          displayName: Deploy to Prod
````

---

# 🟩 GitHub Actions Workflow

## 📦 Features

* CI/CD with GitHub
* Branch-based deployment
* Secure secrets handling

---

## ⚙️ YAML (`.github/workflows/databricks.yml`)

```yaml
name: Databricks CI/CD

on:
  push:
    branches:
      - main

jobs:

# 🧪 DEV DEPLOYMENT
  deploy-dev:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Install Databricks CLI
        run: pip install databricks-cli

      - name: Validate Bundle
        run: databricks bundle validate

      - name: Deploy to Dev
        run: databricks bundle deploy --target dev
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_DEV }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_DEV }}

# 🔍 TEST DEPLOYMENT
  deploy-test:
    needs: deploy-dev
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Deploy to Test
        run: databricks bundle deploy --target test
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_TEST }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_TEST }}

# 🚀 PROD DEPLOYMENT
  deploy-prod:
    needs: deploy-test
    runs-on: ubuntu-latest
    environment: production   # 🔐 Manual approval
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Deploy to Prod
        run: databricks bundle deploy --target prod
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_PROD }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_PROD }}
```

---

# 🔐 Secrets Setup

## Azure DevOps

* Store in **Pipeline Variables / Key Vault**

    * `DATABRICKS_TOKEN_DEV`
    * `DATABRICKS_TOKEN_TEST`
    * `DATABRICKS_TOKEN_PROD`

---

## GitHub

* Go to **Settings → Secrets**
* Add:

    * `DATABRICKS_HOST_DEV`
    * `DATABRICKS_TOKEN_DEV`
    * etc.

---

# 🔥 Best Practices

* ✅ Use separate tokens per environment
* ✅ Enable approval for Prod
* ✅ Validate before deploy
* ✅ Use service principals (not personal tokens)
* ✅ Keep bundle.yml environment-agnostic

---

# 🚫 Common Mistakes

* ❌ Hardcoding tokens
* ❌ Skipping validation step
* ❌ No approval gate for Prod
* ❌ Same cluster for all env

---

# 🎯 Pipeline Flow

```text
Code Push → Validate → Dev → Test → Approval → Prod
```

---

# 🏁 Final Takeaway

👉 CI/CD + DAB =

* 🚀 Fully automated deployments
* 🔐 Secure environment handling
* 📦 Production-ready pipelines

---

## 🚀 Want Next?

* 🔬 Service Principal setup for Databricks
* ⚙️ Terraform + DAB integration
* 📊 Monitoring & alerting for pipelines

