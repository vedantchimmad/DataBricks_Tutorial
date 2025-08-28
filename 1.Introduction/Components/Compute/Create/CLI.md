# 💻 Creating an Interactive Cluster in Databricks using CLI  

---

## 🔹 Introduction  
An **interactive cluster** is a compute resource in Databricks that you use for **exploration, development, and running notebooks**.  
You can create such clusters using the **Databricks CLI** by providing the cluster configuration in **JSON format**.  

---

## 🛠️ Prerequisites  
1. ✅ Install **Databricks CLI**  
```bash
   pip install databricks-cli
````

2. ✅ Configure authentication (with a PAT - Personal Access Token)

```bash
   databricks configure --token
```

Enter:

    * **Databricks Host** (e.g., `https://adb-<workspace-id>.<region>.databricks.com`)
    * **Token** (generated from your Databricks workspace).

---

## 📌 Step 1: Create a JSON Cluster Configuration File

Example: `interactive_cluster.json`

```json
{
  "cluster_name": "my-interactive-cluster",
  "spark_version": "13.3.x-scala2.12",
  "node_type_id": "Standard_DS3_v2",
  "autoscale": {
    "min_workers": 2,
    "max_workers": 4
  },
  "autotermination_minutes": 60,
  "spark_conf": {
    "spark.databricks.cluster.profile": "singleNode"
  }
}
```

🔍 Key fields:

* `cluster_name` → Name of the cluster.
* `spark_version` → Databricks runtime version (use `databricks clusters spark-versions` to list).
* `node_type_id` → VM/instance type (use `databricks clusters list-node-types`).
* `autoscale` → Auto-scaling range of workers.
* `autotermination_minutes` → Auto-shutdown after inactivity.
* `spark_conf` → Optional configs (here example shows single-node setting).

---

## 📌 Step 2: Create the Cluster via CLI

```bash
databricks clusters create --json-file interactive_cluster.json
```

👉 This will return a **cluster\_id**. Example:

```json
{
  "cluster_id": "1234-567890-abcd123"
}
```

---

## 📌 Step 3: Check Cluster Status

```bash
databricks clusters get --cluster-id 1234-567890-abcd123
```

👉 Output will show cluster state (`PENDING`, `RUNNING`, `TERMINATED`).

---

## 📌 Step 4: List Clusters

```bash
databricks clusters list
```

---

## 📌 Step 5: Delete (Terminate) Cluster

```bash
databricks clusters permanent-delete --cluster-id 1234-567890-abcd123
```

---

## ✅ Example Workflow

```bash
# 1. Create cluster
databricks clusters create --json-file interactive_cluster.json

# 2. Get cluster details
databricks clusters get --cluster-id <your-cluster-id>

# 3. Run notebooks/jobs on this cluster via UI/CLI/API

# 4. Delete cluster when no longer needed
databricks clusters permanent-delete --cluster-id <your-cluster-id>
```

---

## 🚀 Tip

* Use `databricks clusters list-node-types` to explore available instance types.
* Use `databricks clusters spark-versions` to find compatible Spark versions.
* Keep `autotermination_minutes` enabled to save costs.

---



