# 🔑 Identity & Authentication in Unity Catalog

Identity & Authentication in **Databricks Unity Catalog (UC)** ensures that only **authorized users** and **trusted applications** can access data.  
It integrates with enterprise **Identity Providers (IdPs)** for **secure authentication and SSO**.

---

## 🧩 Key Concepts

### 1. **Identity Provider (IdP) Integration**
- UC integrates with **cloud-native IdPs** and **enterprise identity systems**:
  - **Azure Active Directory (Azure AD)**
  - **AWS IAM Identity Center (formerly SSO)**
  - **Okta**
  - **Ping Identity**
  - **Google Workspace IAM**
- Supports **SAML 2.0** and **OAuth 2.0** protocols.

---

### 2. **Single Sign-On (SSO)**
- Users authenticate with **one set of credentials** (e.g., corporate email).
- Provides **seamless login** to Databricks and UC without separate passwords.
- Reduces password fatigue and improves security.

✅ Example:  
Login to Databricks workspace using **Azure AD** credentials with MFA enabled.

---

### 3. **Multi-Factor Authentication (MFA)**
- Adds an **extra layer of security**:
  - Password + Authenticator App / SMS / Security Key (FIDO2).
- Ensures sensitive data is not exposed even if credentials are compromised.

---

### 4. **Service Principals**
- **Non-human identities** (like applications, ETL pipelines, APIs).
- Used for **automation & integrations** (CI/CD, ML pipelines).
- Authenticated via **client secret** or **OAuth tokens**.

```bash
# Example: Using a service principal to get a Databricks token
databricks configure --aad-token
````

---

### 5. **Personal Access Tokens (PATs)**

* Tokens used for programmatic access to Databricks REST APIs.
* Alternative to username/password.
* Scoped to a **user identity** with expiration dates.

```bash
# Example: Using PAT in API call
curl -H "Authorization: Bearer <DATABRICKS_PAT>" \
     https://<databricks-instance>/api/2.0/clusters/list
```

---

### 6. **SCIM (System for Cross-domain Identity Management)**

* Automates **user and group provisioning** from IdPs.
* Ensures that:

    * New hires automatically get access.
    * Departed employees automatically lose access.
* Keeps identities in sync between IdP and Databricks.

---

### 7. **Identity Federation**

* UC supports **federated login**:

    * A single identity across **multiple cloud accounts**.
    * Example: One Azure AD login works across **Databricks workspaces in AWS, Azure, and GCP**.

---

## 🔐 Identity Flow in Unity Catalog

1. **User/Service** → Attempts login to Databricks workspace.
2. **Redirect** → Sent to Identity Provider (Azure AD, Okta, etc.).
3. **Authentication** → MFA/SSO verification.
4. **Token Issued** → OAuth/SAML token returned.
5. **Access Granted** → User is mapped to UC roles & permissions.

---

## ✅ Summary

* **IdPs** handle authentication (Azure AD, Okta, IAM, Google).
* **SSO & MFA** strengthen login security.
* **Service Principals** secure automation workflows.
* **PATs & OAuth tokens** enable secure API access.
* **SCIM** automates user/group provisioning.
* **Federated Identity** supports multi-cloud Databricks access.

---
