# 🚀 CI/CD with Automated Testing in Databricks (PyTest + DAB)

---

## 🧠 Goal

👉 Build a **production-grade pipeline** that:
- 🧪 Runs tests automatically  
- 📦 Deploys Databricks Asset Bundles (DAB)  
- 🔄 Promotes code: Dev → Test → Prod  

---

# 🏗️ Architecture

```text
Code Push → Run PyTest 🧪 → Validate Bundle 📦 → Deploy (Dev → Test → Prod)
````

---

# 📂 Project Structure

```text id="pj3ykt"
project/
│
├── src/
│   └── transformations.py
├── tests/
│   └── test_transformations.py
├── bundle.yml
└── .github/workflows/databricks.yml
```

---

# 🧪 Step 1: Add PyTest

```bash
pip install pytest
```

---

# 🧩 Step 2: Sample Test

```python id="j1gn5k"
def test_example():
    assert 1 + 1 == 2
```

---

# ⚙️ Step 3: GitHub Actions CI/CD Pipeline

```yaml
name: Databricks CI/CD with Testing

on:
  push:
    branches:
      - main

jobs:

# 🧪 STEP 1: RUN TESTS
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9'

      - name: Install Dependencies
        run: |
          pip install pytest pyspark

      - name: Run Tests
        run: pytest tests/

# 📦 STEP 2: VALIDATE BUNDLE
  validate:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Install Databricks CLI
        run: pip install databricks-cli

      - name: Validate Bundle
        run: databricks bundle validate

# 🚀 STEP 3: DEPLOY TO DEV
  deploy-dev:
    needs: validate
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Dev
        run: databricks bundle deploy --target dev
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_DEV }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_DEV }}

# 🔍 STEP 4: DEPLOY TO TEST
  deploy-test:
    needs: deploy-dev
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Test
        run: databricks bundle deploy --target test
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_TEST }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_TEST }}

# 🚀 STEP 5: DEPLOY TO PROD (Manual Approval)
  deploy-prod:
    needs: deploy-test
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy to Prod
        run: databricks bundle deploy --target prod
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST_PROD }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN_PROD }}
```

---

# 🟦 Azure DevOps Version

```yaml id="x0r0re"
trigger:
  branches:
    include:
      - main

stages:

# 🧪 TEST
- stage: Test
  jobs:
    - job: RunTests
      steps:
        - script: pip install pytest pyspark
        - script: pytest tests/

# 📦 VALIDATE
- stage: Validate
  dependsOn: Test
  jobs:
    - job: ValidateBundle
      steps:
        - script: pip install databricks-cli
        - script: databricks bundle validate

# 🚀 DEPLOY DEV
- stage: Dev
  dependsOn: Validate
  jobs:
    - job: DeployDev
      steps:
        - script: databricks bundle deploy --target dev

# 🔍 DEPLOY TEST
- stage: TestDeploy
  dependsOn: Dev
  jobs:
    - job: DeployTest
      steps:
        - script: databricks bundle deploy --target test

# 🚀 DEPLOY PROD
- stage: Prod
  dependsOn: TestDeploy
  jobs:
    - job: DeployProd
      steps:
        - script: databricks bundle deploy --target prod
```

---

# 🔐 Secrets Management

## GitHub

* `DATABRICKS_HOST_DEV`
* `DATABRICKS_TOKEN_DEV`

## Azure DevOps

* Store in **Pipeline Variables / Key Vault**

---

# 🔥 Best Practices

* ✅ Fail pipeline if tests fail
* ✅ Keep tests fast (mock data)
* ✅ Separate test & deploy stages
* ✅ Use approval gate for Prod
* ✅ Use service principals

---

# 🚫 Common Mistakes

* ❌ Deploying without tests
* ❌ Skipping validation step
* ❌ Hardcoding credentials
* ❌ No environment separation

---

# 🎯 Pipeline Flow

```text id="w5c23n"
Push Code → Run Tests 🧪 → Validate 📦 → Deploy Dev → Test → Approval → Prod
```

---

# 🏁 Final Takeaway

👉 CI/CD with testing =

* 🧪 Reliable pipelines
* 🚀 Safe deployments
* 🔄 Automated workflow

---
