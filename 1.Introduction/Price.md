# How Cost is Decided in Databricks

Databricks pricing is based on a **pay-as-you-go model**.  
The total cost depends on a combination of **compute usage, storage, and additional features** like Unity Catalog or Workflows.  

---

## 🔹 Key Cost Components

### 1. **Compute (Primary Cost Driver)**
- Compute is charged in **DBUs (Databricks Units)**.
- **DBU = unit of processing capability per hour**.
- Each cluster type (all-purpose, job, SQL, ML, photon) has a specific **DBU rate**.
- Total compute cost = **DBU rate × number of DBUs × cluster run time × cloud VM cost**.

**Example:**
- A job cluster with 4 worker nodes runs for 2 hours.  
- DBU rate = 0.55 per node/hour.  
- Cloud VM cost (AWS EC2, Azure VM, or GCP) is billed separately.  
- Total = (0.55 × 4 × 2) DBUs + VM charges.

---

### 2. **Cloud Infrastructure Cost**
- Databricks runs on top of AWS, Azure, or GCP.  
- You pay **directly to the cloud provider** for:
  - Virtual machines (compute instances).
  - Object storage (S3, ADLS, GCS).
  - Networking, data transfer, etc.

---

### 3. **Storage Costs**
- Data stored in **Delta Lake** (cloud object storage).  
- Billed by your cloud provider.  
- Includes:
  - **Raw data storage**.
  - **Delta log files**.
  - **Snapshots and checkpoints**.  

---

### 4. **Workflows & Jobs**
- Charged based on compute resources used by jobs.  
- If scheduled workflows run frequently, compute costs increase.  
- No extra fee for scheduling — only compute charges apply.

---

### 5. **Serverless SQL Warehouses**
- **On-demand compute** option for BI/SQL workloads.  
- Cost = **per-second billing** of warehouse size (small/medium/large).  
- Includes **DBUs + underlying VM costs**.  

---

### 6. **Unity Catalog & Governance**
- Unity Catalog (UC) itself does not have a separate fee.  
- The cost comes from compute used while **querying governed data**.  

---

### 7. **Premium Features**
- **Photon (accelerated compute)** → same DBU usage but faster, lowering cost per workload.  
- **Delta Live Tables (DLT)** → charged by DBUs consumed during pipeline runs.  
- **Model Serving** → per-second cost based on compute used for serving ML models.  

---

## 🔹 Formula for Cost Estimation

```
Total Cost = (DBU rate × DBUs × runtime) + (Cloud VM cost × runtime) + (Storage cost) + (Optional features)
```

---

## 🔹 Example Breakdown

| Component            | Example Usage                          | Approx. Cost Basis               |
|----------------------|----------------------------------------|----------------------------------|
| Job Cluster          | 4 nodes × 2 hrs                        | DBUs × runtime + VM cost         |
| All-Purpose Cluster  | Interactive analysis 3 hrs/day         | DBUs × runtime + VM cost         |
| SQL Warehouse        | Medium warehouse 1 hr/day              | DBUs/hr + VM cost (per second)   |
| Storage (Delta Lake) | 2 TB on S3                             | Cloud storage pricing (per GB)   |
| DLT Pipeline         | Daily ETL run 1 hr                     | DBUs × runtime                   |

---

## 🔹 Cost Optimization Best Practices

1. **Use Job Clusters instead of All-Purpose Clusters** for scheduled workloads.  
2. **Enable Auto-Termination** to shut down idle clusters.  
3. **Use Spot/Preemptible Instances** (AWS/GCP) or low-priority VMs (Azure) for non-critical jobs.  
4. **Right-size clusters** → avoid over-provisioning nodes.  
5. **Photon Acceleration** → finish workloads faster with same DBU rate.  
6. **Cache & Optimize Queries** in SQL Warehouses to reduce compute.  

---

## 🔹 Summary

- Cost in Databricks is mainly decided by **compute usage (DBUs × runtime)**.  
- Additional costs come from **cloud VM charges, storage, and premium features**.  
- Unity Catalog and Workflows do not add extra fees, only compute usage.  
- Optimizing **cluster usage, auto-termination, and Photon** can significantly reduce costs.  

