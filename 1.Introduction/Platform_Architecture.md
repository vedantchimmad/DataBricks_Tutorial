# Databricks Platform Architecture

Databricks integrates with cloud services to provide a **unified analytics platform** for big data and AI.  
The platform architecture is divided into two main planes: **Control Plane** and **Compute Plane**.

---

## 🔹 High-Level Architecture (Databricks - Detailed)

```text
+-----------------------------------------------------------------------+
|                👨‍💻 Users / Clients                                   |
|  - Web UI (Notebooks, SQL Editor)                                     |
|  - REST APIs                                                          |
|  - BI Tools (Power BI, Tableau)                                       |
|  - CLI / SDK                                                          |
+-----------------------------------------------------------------------+
            | Authentication (SSO, IAM, Tokens)
            ▼
+-----------------------------------------------------------------------+
|        🟣 Databricks Control Plane (Managed by Databricks)            |
|  - Workspace UI                                                       |
|  - Notebooks & Repos                                                  |
|  - Jobs & Workflows                                                   |
|  - Cluster Management                                                 |
|  - Unity Catalog (Governance Layer)                                   |
|  - Access Control & Authentication                                    |
+-----------------------------------------------------------------------+
            | Sends execution requests, metadata, configs
            ▼
+-----------------------------------------------------------------------+
|        🔵 Compute Plane (Customer Cloud - VNet/VPC)                   |
|                                                                       |
|   ⚙️ Clusters / SQL Warehouses                                        |
|   - Driver Node (Execution Coordinator)                               |
|   - Worker Nodes (Parallel Processing)                                |
|                                                                       |
|   📦 Execution Components                                             |
|   - Spark Executors                                                   |
|   - Task Execution                                                    |
|   - Shuffle & Caching                                                 |
|                                                                       |
|   📂 Intermediate Storage                                             |
|   - DBFS (Databricks File System)                                     |
|   - Logs (Driver / Executor logs)                                     |
|   - Temp data / Shuffle data                                          |
+-----------------------------------------------------------------------+
            | Read / Write Data (Secure Access)
            ▼
+-----------------------------------------------------------------------+
|        🟢 Storage Layer (Customer Managed Data Lake)                  |
|                                                                       |
|   - Delta Lake Tables                                                 |
|   - Parquet / CSV / JSON Files                                        |
|   - Managed & External Tables                                         |
|                                                                       |
|   📌 Storage Options                                                  |
|   - Azure Data Lake Storage (ADLS Gen2)                               |
|   - AWS S3                                                            |
|   - Google Cloud Storage                                              |
+-----------------------------------------------------------------------+
            | External Connectors / Federation
            ▼
+-----------------------------------------------------------------------+
|        🌐 External Data Sources                                       |
|                                                                       |
|   - RDBMS (PostgreSQL, MySQL, SQL Server)                             |
|   - Data Warehouses (Snowflake, Redshift)                             |
|   - Streaming Sources (Kafka, Event Hub)                              |
+-----------------------------------------------------------------------+
```
---

## 🔹 Key Components

| Component                         | Description                                                                                                               |
| --------------------------------- |---------------------------------------------------------------------------------------------------------------------------|
| **Control Plane**                 | Managed by Databricks. Hosts the web app, notebooks, job scheduling, and cluster manager.                                 |
| **Compute Plane**                 | Runs inside the customer’s Cloud subscription (VNet). Hosts clusters, SQL warehouses, and handles actual data processing. |
| **DBFS (Databricks File System)** | A distributed file system in the compute plane for storing notebooks, libraries, and logs.                                |
| **Customer Data Storage**         | Azure Data Lake Storage Gen2 (ADLS) or Azure Blob Storage for customer-managed data.                                      |
| **Workspace Root Storage**        | Stores system data, libraries, and root DBFS.                                                                             |
| **External Data Sources**         | Any external system (databases, APIs, streaming platforms) integrated into Databricks.                                    |

---

## 🔹 Two Types of Compute

1. **Serverless Compute Plane**

    * Used for serverless SQL warehouses and Model Serving.
    * Automatically managed by Databricks.

2. **Classic Compute Plane (VNet)**

    * Used for clusters and professional SQL warehouses.
    * Deployed inside the customer’s Azure subscription (VNet).
    * Provides more control and network security.

---

## 🔹 Data Flow in Azure Databricks

1. 👤 User logs into **Databricks Web Application** (Control Plane).
2. ⚙️ Control plane manages jobs, queries, notebooks, and clusters.
3. 🚀 Jobs and clusters are launched in the **Compute Plane** (customer’s VNet).
4. 📂 Data is read/written from **ADLS Gen2 / Blob Storage**.
5. 📡 External data sources can also be connected.
6. 📊 Logs, metadata, and results flow back to the Control Plane.

---

## ✅ Summary

* **Control Plane** → Managed by Databricks (UI, jobs, metadata, orchestration).
* **Compute Plane** → Runs in customer’s Azure subscription (clusters, SQL warehouses).
* **Storage** → Customer-managed (ADLS Gen2 / Blob Storage).
* **Separation** → Ensures **security, scalability, and compliance**.

