[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure DevOps Terraform Pipeline Documentation

## Overview

This pipeline is designed to deploy Azure infrastructure using Terraform in a controlled and enterprise-ready way.

It follows a **two-stage approach**:

1. **Plan Stage** → Generates and stores the execution plan  
2. **Apply Stage** → Executes the previously approved plan  

This ensures:
- Consistency between plan and apply
- Auditability
- Safe deployments

---

## Global Configuration

### Trigger
```yaml
trigger:
- main
```
The pipeline runs automatically on every commit to the `main` branch.

---

### Agent Pool
```yaml
pool:
  vmImage: ubuntu-latest
```
Uses a Microsoft-hosted Ubuntu agent.

---

### Variables

These variables define the Terraform backend and tool versions:

- **TF_STORAGE_ACCOUNT** → Storage Account for Terraform state
- **TF_CONTAINER** → Blob container where state is stored
- **TF_KEY** → State file name
- **TF_RESOURCE_GROUP** → Resource group containing the storage account
- **TF_VERSION** → Terraform CLI version
- **TF_PROVIDER_VERSION** → AzureRM provider version

---

# 🧱 Stage 1 — PLAN

This stage prepares the infrastructure plan but does NOT deploy anything.

---

## Step 1 — Checkout Repository
```yaml
- checkout: self
```
Downloads the Terraform code from the repository into the pipeline agent.

---

## Step 2 — Install Terraform
```yaml
- task: TerraformInstaller@1
```
Installs the required Terraform version to ensure consistency across environments.

---

## Step 3 — Azure CLI Task (Core Logic)

This is the most important step. It performs several actions:

---

### 🔐 Authentication (OIDC)

```bash
export ARM_CLIENT_ID="$servicePrincipalId"
export ARM_TENANT_ID="$tenantId"
export ARM_SUBSCRIPTION_ID=$(az account show --query id -o tsv)
export ARM_OIDC_TOKEN="$idToken"
export ARM_USE_OIDC=true
export ARM_USE_AZUREAD=true
unset ARM_CLIENT_SECRET
```

Purpose:
- Authenticates Terraform using **OIDC (federated identity)**
- Avoids storing secrets
- Uses Azure AD for secure access

---

### 📦 Provider Fix (Offline Installation)

```bash
curl ... terraform-provider-azurerm
```

Purpose:
- Downloads the AzureRM provider manually
- Stores it locally in a plugin cache
- Avoids provider download issues or network restrictions

---

### ⚙️ Terraform Init

```bash
terraform init -reconfigure ...
```

Purpose:
- Initializes Terraform
- Configures remote backend (Azure Storage)
- Enables Azure AD authentication for state access

---

### 📊 Terraform Plan

```bash
terraform plan -out=tfplan_dir/tfplan
```

Purpose:
- Generates an execution plan
- Saves it to a file (`tfplan`)
- Ensures Apply uses EXACTLY the same plan

---

### 🔍 Debug

```bash
ls -la tfplan_dir
```

Purpose:
- Verifies that the plan file was successfully created

---

## Step 4 — Publish Artifact

```yaml
- task: PublishPipelineArtifact@1
```

Purpose:
- Stores the Terraform plan as a pipeline artifact
- Makes it available to the Apply stage
- Ensures immutability between stages

---

# 🚀 Stage 2 — APPLY

This stage executes the previously generated plan.

---

## Step 1 — Checkout Repository
```yaml
- checkout: self
```

Same as in Plan: retrieves Terraform code.

---

## Step 2 — Download Artifact

```yaml
- task: DownloadPipelineArtifact@2
```

Purpose:
- Downloads the Terraform plan from the Plan stage
- Stores it in a dedicated folder (`/artifacts`)
- Avoids permission conflicts in the root workspace

---

## Step 3 — Install Terraform
```yaml
- task: TerraformInstaller@1
```

Ensures the same Terraform version is used.

---

## Step 4 — Azure CLI Task (Execution)

---

### 🔍 Debug Download

```bash
ls -R $(Pipeline.Workspace)
```

Purpose:
- Confirms the plan file exists
- Helps troubleshoot path issues

---

### 🔐 Authentication (Same as Plan)

Ensures Terraform can access Azure resources securely using OIDC.

---

### 📦 Provider Fix

Same as Plan stage:
- Ensures the correct provider is available locally

---

### ⚙️ Terraform Init

Re-initializes Terraform with backend configuration to:
- Access the same state
- Ensure consistency

---

### 🚀 Terraform Apply

```bash
terraform apply "$(Pipeline.Workspace)/artifacts/tfplan"
```

Purpose:
- Executes the previously generated plan
- Guarantees no drift between Plan and Apply
- Ensures deterministic deployment

---

# 🧠 Key Design Decisions

## 1. Plan → Apply Separation
- Prevents unexpected changes
- Enables approvals between stages

## 2. Artifact-Based Deployment
- Ensures the exact same plan is applied
- Improves auditability

## 3. OIDC Authentication
- Eliminates secrets
- Aligns with modern security practices

## 4. Custom Provider Installation
- Avoids network dependency issues
- Ensures deterministic builds

## 5. Workspace Isolation
- Uses `/artifacts` folder to avoid permission conflicts

---

# ⚠️ Important Notes

- The plan file is **binary and environment-specific**
- Do not modify or regenerate it between stages
- Always verify no concurrent Terraform runs exist (state locking)

---

# 🎯 Summary

This pipeline implements a **production-grade Terraform deployment model**:

- Secure (OIDC)
- Controlled (Plan → Apply)
- Reproducible (artifact-based)
- Debuggable (explicit logging)

---
