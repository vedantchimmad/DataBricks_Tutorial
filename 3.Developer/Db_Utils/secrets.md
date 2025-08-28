# 🔐 dbutils.secrets in Databricks  

The **`dbutils.secrets`** utility in Databricks is used to **securely access secrets** (passwords, API keys, tokens, etc.) that are stored in **secret scopes**.  
This helps you avoid hardcoding sensitive information directly in notebooks or jobs.  

---

## ⚙️ Common `dbutils.secrets` Commands  

### 1. **Get a Secret Value**
Fetches the secret value securely.  

```python
dbutils.secrets.get(scope="my-scope", key="db-password")
````

* **scope** → The secret scope name (like a folder).
* **key** → The secret key name inside the scope.
* **return value** → The secret value (but hidden in UI/logs).

---

### 2. **List All Secret Scopes**

Shows all available secret scopes.

```python
dbutils.secrets.listScopes()
```

📌 Example output:

```text
[SecretScope(name='my-scope', backend_type='DATABRICKS')]
```

---

### 3. **List Secrets in a Scope**

Lists all secrets in a specific scope (only keys, not values).

```python
dbutils.secrets.list("my-scope")
```

📌 Example output:

```text
[SecretMetadata(key='db-password', last_updated_timestamp=1692000000000)]
```

---

## 🛠 Example: Secure Database Connection

```python
# Securely fetch credentials
username = dbutils.secrets.get(scope="my-scope", key="db-username")
password = dbutils.secrets.get(scope="my-scope", key="db-password")

# Use them in JDBC connection
jdbc_url = "jdbc:postgresql://myserver:5432/mydb"

df = (spark.read.format("jdbc")
      .option("url", jdbc_url)
      .option("dbtable", "public.sales")
      .option("user", username)
      .option("password", password)
      .load())

df.show()
```

✅ Keeps sensitive info out of code.
✅ Integrates directly with Spark & Databricks.

---

## 🔐 Types of Secret Scopes

| Backend                 | Description                       | Use Case                   |
| ----------------------- | --------------------------------- | -------------------------- |
| **Databricks-backed**   | Managed directly in Databricks    | Small to medium projects   |
| **Azure Key Vault**     | Secrets stored in Azure Key Vault | Enterprise Azure workloads |
| **AWS Secrets Manager** | Uses AWS Secrets Manager          | Enterprise AWS workloads   |
| **GCP Secret Manager**  | Uses GCP Secret Manager           | Enterprise GCP workloads   |

---

## 🚨 Important Notes

* You **cannot print secrets** directly → they are hidden in notebook outputs.
* Use secrets only where needed (avoid exposing in logs).
* Access control can be applied at **scope level** (who can read/write).
* If using external vaults (Azure/AWS/GCP), Databricks automatically integrates.

---

## 📊 Summary

* **`dbutils.secrets.get()`** → Fetch a secret value
* **`dbutils.secrets.listScopes()`** → List all secret scopes
* **`dbutils.secrets.list(scope)`** → List all secrets (keys only) in a scope

✨ With these, you can securely handle **passwords, tokens, and keys** in your workflows without exposing them.
