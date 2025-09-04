# Delta Sharing in Databricks

**Delta Sharing** is an **open protocol** for secure data sharing across organizations, platforms, and clouds.  
It allows you to **share live data** (not copies) with internal teams or external partners **without moving data**.

---

## 🔹 Key Features

- **Open Standard** → based on Delta Sharing protocol, works across platforms.
- **Live Data Sharing** → consumers always access the latest data.
- **Cross-Platform** → can share with any system (Databricks, pandas, Spark, BI tools, etc.).
- **Secure & Governed** → integrates with Unity Catalog for access control.
- **No Data Movement** → consumers query directly from the provider’s cloud storage.

---

## 🔹 Components of Delta Sharing

1. **Provider**
   - The entity that owns the data.
   - Shares tables, views, or datasets via Unity Catalog.

2. **Recipient**
   - The entity (internal/external) that consumes the shared data.
   - Accesses shared data using Delta Sharing client or directly from Databricks.

3. **Share**
   - A collection of **tables** that a provider makes available.
   - Example: `sales_share` containing multiple tables.

4. **Recipient Token (Credential File)**
   - Secure credential file (`.json`) given to the recipient.
   - Contains an **endpoint URL** and **authentication token**.

5. **Unity Catalog Integration**
   - Provides fine-grained access control, auditing, and governance.

---

## 🔹 How Delta Sharing Works

1. **Provider Creates a Share**
   ```sql
   CREATE SHARE sales_share;
   GRANT SELECT ON TABLE sales_db.transactions TO SHARE sales_share;
   ````

2. **Provider Creates a Recipient**

   ```sql
   CREATE RECIPIENT partner_company USING TOKEN;
   ```

3. **Provider Grants Access**

   ```sql
   GRANT SELECT ON SHARE sales_share TO RECIPIENT partner_company;
   ```

4. **Recipient Uses Credential File**

    * Receives `.json` file containing:

      ```json
      {
        "shareCredentialsVersion":1,
        "endpoint":"https://sharing.delta.io/delta-sharing/",
        "bearerToken":"<secure-token>"
      }
      ```

5. **Recipient Reads Data**

    * Using Python (pandas):

      ```python
      import delta_sharing
 
      # Path to credential file
      profile_file = "partner_company_profile.json"
 
      # Load table
      url = f"{profile_file}#sales_share.sales_db.transactions"
      df = delta_sharing.load_as_pandas(url)
 
      print(df.head())
      ```

    * Using Apache Spark:

      ```python
      df = spark.read.format("deltaSharing").load(
          "partner_company_profile.json#sales_share.sales_db.transactions"
      )
      df.show()
      ```

---

## 🔹 Supported Clients

* **Databricks** (directly via Unity Catalog).
* **Apache Spark**.
* **pandas** / **PySpark**.
* **BI Tools** → Tableau, Power BI, Looker, etc.

---

## 🔹 Security & Governance

* **Unity Catalog** manages permissions.
* Providers can **revoke access at any time**.
* Data is always accessed via **short-lived tokens**.
* **Audit logs** track all access events.

---

## 🔹 Example Use Case

* A **retail company** shares daily sales data with its **suppliers** using Delta Sharing.
* Suppliers connect directly from **Tableau** or **Python** without needing access to the company’s Databricks workspace.

---

## 🔹 Summary Table

| Component     | Role                                 |
| ------------- | ------------------------------------ |
| Provider      | Owns and shares data                 |
| Recipient     | Consumes shared data                 |
| Share         | Collection of shared tables          |
| Token File    | Secure access credentials            |
| Unity Catalog | Security, governance, access control |

---

✅ **Delta Sharing** = A secure, open, and real-time way to share **live data** across any platform without duplication.
