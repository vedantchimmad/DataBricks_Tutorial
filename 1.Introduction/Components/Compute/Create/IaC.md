# ⚡ Creating a Databricks SQL Warehouse (Serverless Cluster) using IaC (Infrastructure as Code)

---

## 🔹 What is IaC in Databricks?  
Infrastructure as Code (**IaC**) allows you to define and manage Databricks resources like **clusters, SQL warehouses, jobs, and workspaces** using code instead of manual UI steps.  
Common IaC tools used with Databricks:  
- 🟢 **Terraform** (most popular)  
- 🟢 **Pulumi**  
- 🟢 **Databricks CLI + JSON/YAML templates**  

Here we’ll use **Terraform** since it is officially supported by Databricks.  

---

## 🛠️ Prerequisites  
1. Install Terraform → [Download](https://developer.hashicorp.com/terraform/downloads)  
2. Install Databricks Terraform provider →  

```bash
terraform init
````

3. Generate a **Databricks Personal Access Token (PAT)** from your workspace.
4. Export environment variables:

```bash
export DATABRICKS_HOST="https://adb-<workspace-id>.<region>.databricks.com"
export DATABRICKS_TOKEN="dapixxxxxxxxxxxxx"
```

---

## 📌 Step 1: Terraform Configuration for SQL Warehouse

Create a file **`main.tf`**:

```hcl
terraform {
  required_providers {
    databricks = {
      source  = "databricks/databricks"
      version = ">=1.20.0"
    }
  }
}

provider "databricks" {
  host  = var.databricks_host
  token = var.databricks_token
}

resource "databricks_sql_endpoint" "serverless_sql_wh" {
  name                       = "serverless-sql-warehouse"
  cluster_size               = "Small"                # Options: XSmall, Small, Medium, Large
  max_num_clusters           = 1
  enable_serverless_compute  = true                   # ✅ Enables Serverless
  auto_stop_mins             = 15                     # Auto shutdown after idle
  spot_instance_policy       = "COST_OPTIMIZED"       # Optimize cost using spot instances
  enable_photon              = true                   # Accelerate queries
  tags = {
    Environment = "Dev"
    Owner       = "DataTeam"
  }
}
```

---

## 📌 Step 2: Variables (optional)

Create a **`variables.tf`**:

```hcl
variable "databricks_host" {
  description = "Databricks workspace URL"
  type        = string
}

variable "databricks_token" {
  description = "Databricks PAT Token"
  type        = string
}
```

---

## 📌 Step 3: Terraform Commands

```bash
terraform init       # Initialize provider
terraform plan       # Preview changes
terraform apply -auto-approve   # Create SQL Warehouse
```

---

## 📌 Step 4: Verify

Once applied, you’ll see output like:

```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

databricks_sql_endpoint.serverless_sql_wh: Creation complete
```

You can now:

* See the **SQL Warehouse** in the Databricks **SQL > Warehouses** UI.
* Connect using **JDBC/ODBC** or via **Databricks SQL Editor**.

---

## 🖼️ Visual Flow

```
Terraform (IaC Code) ──► Databricks Provider ──► Creates SQL Warehouse
```

---

## ✅ Benefits of IaC for SQL Warehouses

* 🔄 Reproducible → Same config across Dev/Test/Prod.
* 📜 Version Controlled → Store Terraform in GitHub/GitLab.
* ⚡ Automated Scaling → Auto-stop & cost optimization.
* 🔒 Governance → Secure & auditable.

---

👉 This creates a **serverless SQL Warehouse** (Databricks-managed, no manual cluster needed).
