[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure DevOps Terraform Pipeline Documentation

## Sample Azure Pipeline definition (YAML)
````yml
# =========================================
# PIPELINE TRIGGER
# =========================================
# The pipeline is triggered automatically on every push to the main branch
trigger:
- main

# =========================================
# AGENT POOL
# =========================================
# Uses a Microsoft-hosted Ubuntu agent to run the pipeline
pool:
  vmImage: ubuntu-latest

# =========================================
# GLOBAL VARIABLES
# =========================================
# These variables define:
# - Terraform backend configuration (Azure Storage)
# - Terraform CLI version
# - AzureRM provider version
variables:
  TF_STORAGE_ACCOUNT: 'sttfstatealzlabweu01'   # Storage account used to store Terraform state
  TF_CONTAINER: 'tfstate'                     # Blob container for the state file
  TF_KEY: 'mgmt-groups.tfstate'              # Name of the Terraform state file
  TF_RESOURCE_GROUP: 'rg-tfstate-weu-01'     # Resource group where the storage account exists
  TF_VERSION: '1.6.0'                        # Terraform CLI version
  TF_PROVIDER_VERSION: '4.70.0'              # AzureRM provider version

stages:

# =========================
# PLAN STAGE
# =========================
# This stage generates the Terraform execution plan but does NOT apply it
- stage: Plan
  jobs:
  - job: plan
    steps:

    # Checkout repository containing Terraform code
    - checkout: self

    # Install Terraform CLI (required to run Terraform commands)
    - task: TerraformInstaller@1
      inputs:
        terraformVersion: '$(TF_VERSION)'

    # Main execution block using Azure CLI
    - task: AzureCLI@2
      inputs:
        azureSubscription: 'sc-azure-terraform'  # Service connection used for authentication
        scriptType: bash
        scriptLocation: inlineScript
        addSpnToEnvironment: true                # Exposes SPN details as environment variables
        inlineScript: |
          set -e   # Exit immediately if any command fails

          echo "===== AUTH ====="
          # Configure Terraform authentication using OIDC (no secrets required)
          export ARM_CLIENT_ID="$servicePrincipalId"
          export ARM_TENANT_ID="$tenantId"
          export ARM_SUBSCRIPTION_ID=$(az account show --query id -o tsv)
          export ARM_OIDC_TOKEN="$idToken"
          export ARM_USE_OIDC=true
          export ARM_USE_AZUREAD=true
          unset ARM_CLIENT_SECRET   # Ensure no secret-based auth is used

          echo "===== FIX PROVIDER ====="
          # Manual installation of AzureRM provider (used when avoiding direct downloads)
          # This is optional in standard environments (Terraform can download providers automatically)
          PLUGIN_PATH="$HOME/.terraform.d/plugins"

          mkdir -p $PLUGIN_PATH/registry.terraform.io/hashicorp/azurerm/$(TF_PROVIDER_VERSION)/linux_amd64

          curl -sL -o azurerm.zip \
          https://releases.hashicorp.com/terraform-provider-azurerm/$(TF_PROVIDER_VERSION)/terraform-provider-azurerm_$(TF_PROVIDER_VERSION)_linux_amd64.zip

          unzip -o azurerm.zip -d $PLUGIN_PATH/registry.terraform.io/hashicorp/azurerm/$(TF_PROVIDER_VERSION)/linux_amd64

          # Configure Terraform to use the local provider mirror instead of downloading it
          cat > ~/.terraformrc <<EOF
          provider_installation {
            filesystem_mirror {
              path    = "$PLUGIN_PATH"
              include = ["registry.terraform.io/hashicorp/azurerm"]
            }
            direct {
              exclude = ["registry.terraform.io/hashicorp/azurerm"]
            }
          }
          EOF

          echo "===== INIT ====="
          # Initialize Terraform and configure backend
          # -reconfigure forces Terraform to ignore previous backend config and reload it
          terraform init -reconfigure \
            -backend-config="resource_group_name=$(TF_RESOURCE_GROUP)" \
            -backend-config="storage_account_name=$(TF_STORAGE_ACCOUNT)" \
            -backend-config="container_name=$(TF_CONTAINER)" \
            -backend-config="key=$(TF_KEY)" \
            -backend-config="use_azuread_auth=true"

          echo "===== PLAN ====="
          # Generate Terraform execution plan and store it in a file
          # This ensures the same plan is used later in the Apply stage
          mkdir -p tfplan_dir
          terraform plan -var-file="terraform.tfvars" -out=tfplan_dir/tfplan

          echo "===== DEBUG PLAN ====="
          # Verify that the plan file was created successfully
          ls -la tfplan_dir

    # Publish the Terraform plan as an artifact to be used in the Apply stage
    - task: PublishPipelineArtifact@1
      inputs:
        targetPath: 'tfplan_dir'   # Folder containing the plan file
        artifact: 'tfplan'         # Artifact name


# =========================
# APPLY STAGE
# =========================
# This stage applies the previously generated Terraform plan
- stage: Apply
  dependsOn: Plan   # Ensures Apply only runs after Plan completes
  jobs:
  - deployment: apply
    environment: prod   # Enables environment-based approvals if configured
    strategy:
      runOnce:
        deploy:
          steps:

          # Checkout repository again (needed for Terraform configuration files)
          - checkout: self

          # Download the Terraform plan artifact from the Plan stage
          # Stored in a subfolder to avoid permission conflicts
          - task: DownloadPipelineArtifact@2
            inputs:
              artifact: 'tfplan'
              path: '$(Pipeline.Workspace)/artifacts'

          # Install Terraform CLI again (ensures same version as Plan stage)
          - task: TerraformInstaller@1
            inputs:
              terraformVersion: '$(TF_VERSION)'

          # Execute Terraform Apply
          - task: AzureCLI@2
            inputs:
              azureSubscription: 'sc-azure-terraform'
              scriptType: bash
              scriptLocation: inlineScript
              addSpnToEnvironment: true
              inlineScript: |
                set -e

                echo "===== DEBUG DOWNLOAD ====="
                # Verify that the artifact was downloaded correctly
                ls -R $(Pipeline.Workspace)

                echo "===== AUTH ====="
                # Reconfigure authentication (same as Plan stage)
                export ARM_CLIENT_ID="$servicePrincipalId"
                export ARM_TENANT_ID="$tenantId"
                export ARM_SUBSCRIPTION_ID=$(az account show --query id -o tsv)
                export ARM_OIDC_TOKEN="$idToken"
                export ARM_USE_OIDC=true
                export ARM_USE_AZUREAD=true
                unset ARM_CLIENT_SECRET

                echo "===== FIX PROVIDER ====="
                # Same provider installation logic to ensure consistency with Plan stage
                PLUGIN_PATH="$HOME/.terraform.d/plugins"

                mkdir -p $PLUGIN_PATH/registry.terraform.io/hashicorp/azurerm/$(TF_PROVIDER_VERSION)/linux_amd64

                curl -sL -o azurerm.zip \
                https://releases.hashicorp.com/terraform-provider-azurerm/$(TF_PROVIDER_VERSION)/terraform-provider-azurerm_$(TF_PROVIDER_VERSION)_linux_amd64.zip

                unzip -o azurerm.zip -d $PLUGIN_PATH/registry.terraform.io/hashicorp/azurerm/$(TF_PROVIDER_VERSION)/linux_amd64

                cat > ~/.terraformrc <<EOF
                provider_installation {
                  filesystem_mirror {
                    path    = "$PLUGIN_PATH"
                    include = ["registry.terraform.io/hashicorp/azurerm"]
                  }
                  direct {
                    exclude = ["registry.terraform.io/hashicorp/azurerm"]
                  }
                }
                EOF

                echo "===== INIT ====="
                # Re-initialize Terraform to connect to the same backend
                terraform init -reconfigure \
                  -backend-config="resource_group_name=$(TF_RESOURCE_GROUP)" \
                  -backend-config="storage_account_name=$(TF_STORAGE_ACCOUNT)" \
                  -backend-config="container_name=$(TF_CONTAINER)" \
                  -backend-config="key=$(TF_KEY)" \
                  -backend-config="use_azuread_auth=true"

                echo "===== APPLY ====="
                # Apply the EXACT plan generated in the Plan stage
                # This guarantees deterministic deployments
                terraform apply "$(Pipeline.Workspace)/artifacts/tfplan"

````

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
This task installs the **Terraform CLI binary**, which is required to run:

- `terraform init`
- `terraform plan`
- `terraform apply`

👉 This is **NOT related to providers**. It only installs the Terraform executable.

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

Terraform normally downloads providers automatically during:

```bash
terraform init
```

However, in this pipeline we are **overriding the default behavior** by:

- Downloading the provider manually
- Storing it in a local plugin cache
- Forcing Terraform to use a **filesystem mirror**

```hcl
provider_installation {
  filesystem_mirror {
    path = "$PLUGIN_PATH"
  }
}
```
### ⚠️ Important Clarification

- **Terraform CLI ≠ Terraform Provider**
- CLI is installed via `TerraformInstaller`
- Provider is the plugin (`azurerm`) downloaded manually


---
---

### ❗ Why this is done

This approach is useful in:

- Restricted environments (no internet access)
- Enterprise environments with strict compliance
- Scenarios where provider versions must be tightly controlled

---

### ❗ When this is NOT needed

In most standard environments, this whole block can be removed and replaced with:

```bash
terraform init
```

👉 Terraform will automatically:
- Download providers
- Cache them
- Manage versions

---

### ⚠️ Recommendation

If you are in a **normal environment with internet access**, this manual provider installation:

- Adds complexity
- Is not required
- Can be safely removed

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
