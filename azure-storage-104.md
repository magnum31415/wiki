#  Azure Storage



## Modelos de autenticación en Azure Storage

| Model | Uses Claims | Uses Keys | Example | Related Concept | What it is | Where it is used |
|---|---|---|---|---|---|---|
| Microsoft Entra ID (RBAC) | ✔ | ❌ | A user or application authenticates with Entra ID and accesses a blob using a role like **Storage Blob Data Contributor**. | **Claim** | Information about an identity included in an authentication token. | OAuth / OpenID Connect / Microsoft Entra ID |
| Shared Key (Access Keys) | ❌ | ✔ | An application connects to a storage account using a **connection string with the AccountKey**. | **Access Key (Storage)** | Secret key that provides direct access to the Storage Account. | Key-based authentication |
| SAS Token | ❌ | ✔ | **Shared Access Signature Token** : A temporary URL is generated to allow download of a specific blob for a limited time. | **Access Key (or delegation key)** | Token generated from a storage key or delegation key to grant temporary access. | Delegated access to storage resources |


## Permissions in Azure can be assigned at different scopes within the resource hierarchy.

| Scope | Description | Example |
|---|---|---|
| **Container** | Permissions apply only to a specific container inside a storage account. | A user can read blobs only in `images-container`. |
| **Storage Account** | Permissions apply to all containers and services within a storage account. | A user can manage all blobs in `mystorageaccount`. |
| **Resource Group** | Permissions apply to all resources inside a resource group. | A user can manage the storage account and VMs inside `rg-app-prod`. |
| **Subscription** | Permissions apply to every resource in the subscription. | An administrator can manage all resources in the subscription. |

# Authentication Methods for Azure Storage

## 1. Microsoft Entra ID (RBAC)

Authentication is performed using **identities managed in Microsoft Entra ID**.

Users, applications, or managed identities authenticate with Entra ID and receive a **token containing claims**.  
Azure then checks the **RBAC role assigned to that identity** to determine permissions on the storage account.

### How it works

```
User / Application
        │
        ▼
Microsoft Entra ID authentication
        │
        ▼
Token with claims
        │
        ▼
Azure RBAC evaluates permissions
        │
        ▼
Access to Storage resource
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | Yes |
| Uses tokens | Yes |
| Uses RBAC roles | Yes |
| Supports least privilege | Yes |
| Supports audit and governance | Yes |

Example roles:

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner

---

## 2. Shared Key (Access Keys)

Authentication is performed using a **secret key associated with the storage account**.

Each storage account has two keys:

```
Key1
Key2
```

Any client that has the key can access the storage account.

### How it works

```
Application
      │
      ▼
Uses Storage Account Access Key
      │
      ▼
Azure Storage authenticates request
      │
      ▼
Access granted
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | No |
| Uses tokens | No |
| Uses secret keys | Yes |
| Access scope | Full storage account |
| Security risk | Higher if keys are exposed |

Because the key grants **broad access**, key rotation and protection are critical.

---

## 3. SAS Token (Shared Access Signature)

A **SAS token** provides **delegated and time-limited access** to specific storage resources.

It is generated using either:

- an **Access Key**
- a **User Delegation Key (Entra ID)**

The token specifies:

- allowed operations
- resource scope
- expiration time

### How it works

```
Storage Account
      │
Generate SAS token
      │
      ▼
Client receives token
      │
      ▼
Temporary access to specific resource
```

Example:

```
https://storage.blob.core.windows.net/container/file.txt?sv=...&sig=...
```

### Characteristics

| Feature | Description |
|---|---|
| Identity-based | Optional |
| Time limited | Yes |
| Granular permissions | Yes |
| Scope | Specific container/blob/file |
| Common use case | Temporary sharing |

---

# Comparison

| Method | Identity-based | Security Level | Typical Use |
|---|---|---|---|
| Microsoft Entra ID (RBAC) | Yes | Highest | Applications and services |
| Shared Key | No | Lower | Legacy integrations |
| SAS Token | Partial | Medium | Temporary access |

---

# Recommended Method

The **recommended method is Microsoft Entra ID with RBAC**.

Reasons:

- identity-based authentication
- least privilege access control
- integrates with Azure security policies
- supports auditing and governance
- avoids exposing secret keys

Architecture example:

```
Application
     │
     ▼
Microsoft Entra ID authentication
     │
     ▼
RBAC authorization
     │
     ▼
Azure Storage access
```

---

# Summary

| Method | Recommended |
|---|---|
| Microsoft Entra ID (RBAC) | Yes |
| SAS Token | Only for temporary delegated access |
| Shared Key | Avoid when possible |
