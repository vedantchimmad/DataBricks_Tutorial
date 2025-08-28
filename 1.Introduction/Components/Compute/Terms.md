# 📘 Key Terms in Databricks Compute

Databricks compute is the **engine** that powers all workloads (ETL, ML, SQL, BI).  
Below is a glossary of the **most important terms** used in Databricks Compute:

---

## 🔑 Core Concepts
| Term | Description |
|------|-------------|
| **Cluster** | A set of VMs (driver + workers) running Spark/Databricks Runtime to process data. |
| **Driver Node** | The master node that coordinates tasks, runs the Spark driver program, and communicates with the Databricks control plane. |
| **Worker Node** | The nodes that actually run computations and store partitions of data. |
| **Cluster Mode** | Defines how resources are shared. Examples: **Standard**, **High Concurrency**, **Single Node**. |
| **Interactive Cluster** | Long-running cluster used for notebooks, dev work, and exploration. |
| **Job Cluster** | Ephemeral cluster created on-demand to run scheduled jobs and then auto-terminated. |
| **SQL Warehouse (Classic)** | Compute for SQL queries and dashboards, backed by clusters. |
| **Serverless SQL Warehouse** | Fully managed compute for SQL workloads, Databricks handles scaling/infra. |
| **Photon** | Databricks’ native query execution engine (vectorized, high-performance). |

---

## ⚡ Performance & Scaling
| Term | Description |
|------|-------------|
| **Autoscaling** | Automatically adjusts the number of worker nodes based on workload demand. |
| **Auto-Termination** | Automatically shuts down a cluster after a period of inactivity (to save cost). |
| **Cluster Size** | Determines the VM type and number of workers (e.g., Small, Medium, Large). |
| **Node Type** | The VM/machine specification (e.g., `Standard_DS3_v2`, `m5.xlarge`). |
| **Driver Size** | Specifies the compute size of the driver node. |
| **Worker Size** | Specifies the compute size of worker nodes. |
| **High Concurrency Mode** | Optimized mode for multiple BI users & dashboards. |

---

## 💰 Pricing & Cost
| Term | Description |
|------|-------------|
| **DBU (Databricks Unit)** | A unit of compute usage per hour. Pricing is **DBU × DBU Rate × Hours**. |
| **DBU Rate** | Cost assigned per workload type (Jobs, Interactive, SQL, ML). |
| **VM Cost** | Charged separately by the cloud provider (AWS/Azure/GCP). |
| **Spot Instances** | Discounted VM compute (cheaper but can be preempted). |
| **On-Demand Instances** | Regular priced VM compute (more reliable than Spot). |

---

## 🔒 Security & Governance
| Term | Description |
|------|-------------|
| **Cluster Policies** | Admin-defined rules for creating clusters (e.g., limit size, enforce tags). |
| **IAM Role / Service Principal** | Used for authentication to connect Databricks with cloud storage (S3, ADLS, GCS). |
| **Credential Passthrough** | Lets clusters inherit the identity of users for secure storage access. |
| **Unity Catalog** | Central governance layer for data + compute, controls access to clusters & SQL. |

---

## 🛠️ Compute Management
| Term | Description |
|------|-------------|
| **Databricks Runtime (DBR)** | Pre-configured Spark environment (with ML, GPU, etc.). |
| **Compute Pools** | Pre-warmed compute resources to reduce cluster startup time. |
| **Node Pools** | Group of VMs available for fast cluster provisioning. |
| **Ephemeral Cluster** | Temporary cluster created for a job and terminated afterward. |
| **Persistent Cluster** | Always-on cluster (usually interactive). |
| **Cluster ID** | Unique identifier for a cluster (used in CLI/API calls). |

---

## 🧑‍💻 Specialized Compute
| Term | Description |
|------|-------------|
| **Single Node Cluster** | Runs Spark on one machine (driver = worker). Used for ML experimentation. |
| **GPU Cluster** | Cluster using GPU-enabled VMs for deep learning workloads. |
| **ML Runtime** | Specialized Databricks Runtime with preinstalled ML/DL libraries. |
| **Delta Live Tables (DLT) Compute** | Serverless compute for managing streaming/ETL pipelines. |
| **SQL Endpoints** | Compute endpoints for running SQL queries (old term for SQL Warehouses). |

---

## 🔄 Workflows & Jobs
| Term | Description |
|------|-------------|
| **Workflow** | Orchestration of multiple tasks (notebooks, scripts, SQL) using compute. |
| **Task** | An individual job step that runs on a cluster. |
| **Job Run** | Execution instance of a job on a job cluster. |
| **Job Cluster vs All-Purpose Cluster** | Job clusters are created for a single run; All-purpose are long-running. |

---

## 🚀 Compute Lifecycle
| Term | Description |
|------|-------------|
| **Cluster Start/Terminate** | Lifecycle states when compute is active or stopped. |
| **Pending State** | Cluster is initializing resources. |
| **Running State** | Cluster is live and ready to process workloads. |
| **Terminated State** | Cluster has been shut down. |
| **Restarting** | Cluster rebooting due to config changes. |

---

# 📝 Summary
Databricks Compute = **Driver + Workers + Runtime + Scaling + Pricing**.  
👉 Terms revolve around **cluster types**, **scaling features**, **pricing (DBUs)**, and **security controls**.

---