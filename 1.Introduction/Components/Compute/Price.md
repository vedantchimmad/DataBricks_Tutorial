# 💰 Databricks Compute Pricing Overview

Databricks pricing for compute is based on **Databricks Units (DBUs)**, combined with **cloud infrastructure costs** (VMs, storage, etc.).  
The cost you pay = **DBU Price × DBU Hours + Cloud VM Charges**.

---

## ⚡ What is a DBU?
- A **DBU (Databricks Unit)** is a unit of processing capability per hour, billed on a per-second basis.  
- Each cluster type, workload, and compute size consumes DBUs at different rates.  
- You **pay separately for DBUs (to Databricks)** and **VM compute/storage (to your cloud provider: AWS, Azure, GCP)**.

---

## 🖥️ Types of Compute Pricing in Databricks

| Compute Type | Usage | Pricing Basis | Key Notes |
|--------------|-------|---------------|-----------|
| **Interactive Clusters** | Used for **ad-hoc exploration, dev notebooks** | DBUs per node per hour + VM cost | Billed as long as cluster is running |
| **Job Clusters** | **Automated / ephemeral ETL jobs** | DBUs per node per hour + VM cost | Created & terminated with the job |
| **SQL Warehouses** | For **BI dashboards / SQL queries** | DBUs per warehouse per hour + VM cost | Different rates for classic vs serverless |
| **Serverless SQL Warehouses** | Fully managed by Databricks | Only DBU cost (infra included) | Higher DBU price but no VM management |
| **High Concurrency Clusters** | BI tools & concurrent users | DBUs per node per hour + VM cost | Optimized for shared workloads |
| **Single-Node Clusters** | ML/AI experimentation | DBUs + VM cost | Charged as one node only |
| **Photon-Enabled Clusters** | Optimized execution engine | Same DBUs, but faster execution | Reduces total runtime cost |

---

## 🔎 Example Pricing Model

### Case 1: Interactive Cluster on Azure
- Cluster size: **4 workers + 1 driver**  
- VM type: `Standard_DS3_v2` (~$0.30/hr each from Azure)  
- DBU rate (Interactive, Premium tier): **0.55 DBU/hr per node**  
- Total hourly cost ≈  
  `5 nodes × (0.30 cloud cost + 0.55 DBU cost)` = **$4.25/hr**

---

### Case 2: Serverless SQL Warehouse
- Size: **Small (2–4 vCPUs, ~8GB RAM)**  
- DBU rate: **$0.40 – $0.70 / DBU-hour** (varies by tier & region)  
- No VM cost (Databricks manages infra).  
- Total hourly cost ≈ **$0.70 – $2.10/hr** depending on scale-up.  

---

### Case 3: Job Cluster for ETL
- **6 workers + 1 driver**  
- Each VM: ~$0.40/hr (cloud provider)  
- DBU rate (Jobs Compute, Premium): **0.30 DBU/hr per node**  
- Total hourly cost ≈ `7 × (0.40 + 0.30)` = **$4.90/hr**  

---

## ⚙️ Factors Affecting Cost
- **Cluster Type** → Interactive costs more than Job clusters (higher DBU rate).  
- **Tier (Standard / Premium / Enterprise)** → Premium adds governance/security features (higher DBU).  
- **Cloud Provider Region** → VM costs vary by AWS/Azure/GCP and region.  
- **Auto-Termination** → Idle clusters still cost unless terminated.  
- **Photon Acceleration** → Reduces runtime, lowering total spend.  
- **Spot Instances** → Can cut compute VM costs up to 70% (with some risk of eviction).  

---

## 📊 DBU Rate Comparison (Approx, varies by cloud & tier)

| Workload Type | DBU/hr (Standard Tier) | DBU/hr (Premium Tier) |
|---------------|-------------------------|------------------------|
| Jobs Compute  | ~0.15 – 0.30 | ~0.20 – 0.35 |
| Interactive   | ~0.40 – 0.55 | ~0.55 – 0.70 |
| SQL Classic   | ~0.22 – 0.44 | ~0.35 – 0.55 |
| Serverless SQL| ~0.40 – 0.70 | ~0.55 – 0.90 |
| ML Runtime    | ~0.30 – 0.55 | ~0.45 – 0.70 |

---

## 🚀 Cost Optimization Tips
1. ✅ Use **Auto-Termination** for idle clusters.  
2. ✅ Prefer **Job Clusters** for scheduled ETL instead of always-on clusters.  
3. ✅ Use **Serverless SQL** for BI dashboards (pay only per query load).  
4. ✅ Enable **Photon** for query acceleration.  
5. ✅ Use **Spot VMs** where supported to save infra cost.  
6. ✅ Monitor with **Cost Usage Dashboards** in Databricks.  

---