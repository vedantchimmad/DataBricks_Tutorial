# ⚡ Creating a Serverless Compute Cluster in Databricks using Python API  

---

## 🔹 Introduction  
Databricks provides **Serverless Compute (SQL Warehouses / Clusters)**, which automatically manage compute resources.  
Unlike interactive/job clusters, **serverless clusters do not require manual cluster creation** – they are provisioned by Databricks when you create a **SQL Warehouse** or use certain APIs.  

However, if you want to create/manage **clusters programmatically** (including serverless-like configurations), you can use the **Databricks REST API** through Python.  

---

## 🛠️ Prerequisites  
1. Install `databricks-cli` or use the `requests` library in Python.  
2. Generate a **Personal Access Token (PAT)** in your Databricks workspace.  
3. Set your workspace URL and token as environment variables:  

```bash
export DATABRICKS_HOST="https://adb-<workspace-id>.<region>.databricks.com"
export DATABRICKS_TOKEN="dapixxxxxxxxxxxxx"
````

---

## 📌 Step 1: Install Required Libraries

```bash
pip install requests
```

---

## 📌 Step 2: Python Code to Create a Serverless Cluster

```python
import requests
import os
import json

# Workspace details
DATABRICKS_HOST = os.getenv("DATABRICKS_HOST")
DATABRICKS_TOKEN = os.getenv("DATABRICKS_TOKEN")

# API endpoint
url = f"{DATABRICKS_HOST}/api/2.0/clusters/create"

# Cluster payload (serverless-like setup)
payload = {
    "cluster_name": "serverless-cluster-demo",
    "spark_version": "13.3.x-scala2.12",   # Latest runtime
    "node_type_id": "Serverless",          # Special config for serverless
    "autotermination_minutes": 30,
    "num_workers": 0,                      # 0 → fully managed by Databricks
    "data_security_mode": "SINGLE_USER"    # Required for serverless
}

# Headers
headers = {
    "Authorization": f"Bearer {DATABRICKS_TOKEN}"
}

# Send request
response = requests.post(url, headers=headers, data=json.dumps(payload))

# Print response
print(response.status_code)
print(response.json())
```

---

## 📌 Step 3: Check Cluster Status

```python
cluster_id = response.json().get("cluster_id")

status_url = f"{DATABRICKS_HOST}/api/2.0/clusters/get?cluster_id={cluster_id}"
status = requests.get(status_url, headers=headers).json()

print("Cluster State:", status.get("state"))
```

---

## 📌 Step 4: Delete the Cluster (if needed)

```python
delete_url = f"{DATABRICKS_HOST}/api/2.0/clusters/permanent-delete"
delete_payload = {"cluster_id": cluster_id}

delete_resp = requests.post(delete_url, headers=headers, data=json.dumps(delete_payload))
print(delete_resp.json())
```

---

## ✅ Notes on Serverless

* In **AWS & Azure Databricks**, true *serverless* is tied to **Databricks SQL Warehouses**.
* For SQL workloads, you should use the **SQL Warehouses API**:

    * Endpoint: `/api/2.0/sql/warehouses`
    * This automatically gives you **serverless compute** (no manual cluster setup).

👉 The above code shows how to **mimic a serverless cluster** using API by setting `node_type_id` to `"Serverless"` and worker count to `0`.

---

## 🚀 Alternative (Direct SQL Warehouse Creation via API)

If you want a **real serverless SQL Warehouse**:

```python
url = f"{DATABRICKS_HOST}/api/2.0/sql/warehouses"

payload = {
    "name": "serverless-sql-warehouse",
    "cluster_size": "Small",
    "max_num_clusters": 1,
    "enable_serverless_compute": True
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

---
