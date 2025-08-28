# 📂 `dbutils.fs` – File System Utilities in Databricks  

The **`dbutils.fs` API** is used to **interact with the Databricks File System (DBFS)**.  
DBFS is an **abstraction over cloud storage (AWS S3, Azure Blob, GCP Storage)**, allowing you to use a **Unix-like file system API** inside Databricks.  

### 🔹 Important Concepts

- **Source Path (`src`)** → The **file or folder you want to read/move/copy/delete**.
- **Target Path (`dst`)** → The **destination location** where files/folders will be copied/moved.
- **`recurse` Parameter** →
    - `recurse=False` → Works on **single files only**.
    - `recurse=True` → Works on **entire directories and their contents recursively**.
---

## 🔹 Common `dbutils.fs` Commands  

### 1. 📋 **List Files**
```python
dbutils.fs.ls("/mnt/raw")
````

✅ Lists all files & directories inside `/mnt/raw`.

---

### 2. 📁 **Create Directory**

```python
dbutils.fs.mkdirs("/mnt/processed/data")
```

✅ Creates a nested directory if it doesn’t exist.

---

### 3. 📄 **Copy Files**

```python
dbutils.fs.cp("dbfs:/mnt/raw/data.csv", "dbfs:/mnt/processed/data.csv")
```

✅ Copies a file from source to destination.

With recursion:

```python
dbutils.fs.cp("dbfs:/mnt/raw/", "dbfs:/mnt/archive/", recurse=True)
```

---

### 4. 🗑️ **Remove Files/Directories**

```python
dbutils.fs.rm("dbfs:/mnt/processed/data.csv")
```

✅ Deletes a file.

Recursive delete:

```python
dbutils.fs.rm("dbfs:/mnt/archive/", recurse=True)
```

---

### 5. ✏️ **Move (Rename) Files**

```python
dbutils.fs.mv("dbfs:/mnt/raw/data.csv", "dbfs:/mnt/processed/data.csv")
```

✅ Moves or renames a file/directory.

With recursion:

```python
dbutils.fs.mv("dbfs:/mnt/raw/", "dbfs:/mnt/old_raw/", recurse=True)
```

---

### 6. 📖 **Put (Write to a File)**

```python
dbutils.fs.put("/mnt/processed/sample.txt", "Hello, Databricks!")
```

✅ Writes text content to a file (overwrites if exists).

Append mode:

```python
dbutils.fs.put("/mnt/processed/sample.txt", "New line", overwrite=True)
```

---

### 7. 📥 **Head (Preview File Content)**

```python
dbutils.fs.head("/mnt/processed/sample.txt", 100)
```

✅ Reads the first 100 characters of a file.

---

### 8. 🗂️ **Mount External Storage**

```python
dbutils.fs.mount(
  source = "wasbs://mycontainer@myaccount.blob.core.windows.net/",
  mount_point = "/mnt/blobstorage",
  extra_configs = {"fs.azure.account.key.myaccount.blob.core.windows.net":"<key>"}
)
```

✅ Mounts external storage (Azure, AWS, or GCP) to DBFS.

---

### 9. 🔓 **Unmount Storage**

```python
dbutils.fs.unmount("/mnt/blobstorage")
```

---

## 🔎 Example Workflow with `dbutils.fs`

```python
# Step 1: List raw files
files = dbutils.fs.ls("/mnt/raw")
for f in files:
    print(f.name, f.size)

# Step 2: Copy file to processed location
dbutils.fs.cp("dbfs:/mnt/raw/data.csv", "dbfs:/mnt/processed/data.csv")

# Step 3: Preview file content
print(dbutils.fs.head("dbfs:/mnt/processed/data.csv", 200))

# Step 4: Delete raw file after processing
dbutils.fs.rm("dbfs:/mnt/raw/data.csv")
```

---

## 📌 Summary of `dbutils.fs` Commands

| Command                               | Purpose                     |
| ------------------------------------- | --------------------------- |
| `ls(path)`                            | List files in a directory   |
| `mkdirs(path)`                        | Create directory            |
| `cp(src, dst, recurse=False)`         | Copy files/directories      |
| `mv(src, dst, recurse=False)`         | Move/Rename files           |
| `rm(path, recurse=False)`             | Delete files/directories    |
| `put(path, content, overwrite=False)` | Write content to file       |
| `head(path, maxBytes)`                | Preview first bytes of file |
| `mount()`                             | Mount external storage      |
| `unmount(path)`                       | Unmount storage             |

---

💡 Pro Tip: For heavy operations (like large file copies), prefer **Spark APIs** instead of `dbutils.fs` for better performance.
