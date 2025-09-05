# Azure Databricks Platform Architecture

Azure Databricks integrates with Azure services to provide a **unified analytics platform** for big data and AI.  
The platform architecture is divided into two main planes: **Control Plane** and **Compute Plane**.

---

## 🔹 High-Level Architecture

```text
+---------------------------------------------------------------+
|                Customer's Databricks Account                  |
+---------------------------------------------------------------+
         | (Web UI, REST API, SSO with Azure AD)
         ▼
+---------------------------------------------------------------+
|           Azure Databricks Control Plane (Managed)            |
|  - Databricks Web Application                                 |
|  - Notebooks                                                  |
|  - Jobs and Queries                                           |
|  - Cluster Management                                         |
+---------------------------------------------------------------+
         | Upload files / Push & Pull metadata, logs, results
         ▼
+---------------------------------------------------------------+
|        Classic Compute Plane (Customer's VNet)                |
|  Compute resources for clusters & SQL warehouses              |
|                                                               |
|   - DBFS (FileStore, libraries)                               |
|   - Spark logs                                                |
|   - Job results                                               |
|   - Workspace root storage (ADLS Gen2 or Blob Storage)        |
+---------------------------------------------------------------+
         | Direct Access or DBFS mount
         ▼
+---------------------------------------------------------------+
|   Customer Data in ADLS Gen2 or Blob Storage                  |
+---------------------------------------------------------------+
         | External data connectors
         ▼
+---------------------------------------------------------------+
|   External Data Sources                                       |
+---------------------------------------------------------------+
````

---

## 🔹 Key Components

| Component                         | Description                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Control Plane**                 | Managed by Azure Databricks. Hosts the web app, notebooks, job scheduling, and cluster manager.                           |
| **Compute Plane**                 | Runs inside the customer’s Azure subscription (VNet). Hosts clusters, SQL warehouses, and handles actual data processing. |
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

