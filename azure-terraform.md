[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Terraform Troubleshooting Guide (azure-400)

## 🔍 1. Check if a Lock is Active

Before taking any action, verify whether the Terraform state file is currently locked.

Terraform uses Azure Blob leases to lock the state file during operations.

### Option 1 — Basic Check

```bash
az storage blob show \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --name mgmt-groups.tfstate \
  --query "properties.lease"
```

### Option 2 — Using Azure AD Authentication (Recommended)

```bash
az storage blob show \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --name mgmt-groups.tfstate \
  --query "properties.lease" \
  --auth-mode login
```

---

## 🔒 2. Breaking Terraform State Lock (Azure Storage)

When using Azure Storage as a backend for Terraform, the state file can become locked (for example, due to a failed or interrupted run).

If a lock is confirmed and no Terraform process is running, you can safely remove it.

### 🔧 Break the Lease (Remove Lock)

```bash
az storage blob lease break \
  --account-name sttfstatealzlabweu01 \
  --container-name tfstate \
  --blob-name mgmt-groups.tfstate \
  --auth-mode login
```

---

## 🧠 Notes

- Terraform uses blob leases to implement state locking.
- If a lease is active, Terraform operations like `plan` or `apply` will fail.
- Always verify that no other Terraform process is running before breaking the lease.
- Prefer Azure AD authentication (`--auth-mode login`) over storage account keys in enterprise environments.
- Breaking a lease manually should be considered a recovery action, not a normal workflow.
