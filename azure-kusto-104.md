[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Kusto (KQL) Basics for Azure Resource Graph

## 🧠 What is Kusto Query Language (KQL)?

Kusto Query Language (KQL) is the query language used to explore and analyze data in Azure services such as:

- Azure Resource Graph (ARG)
- Log Analytics
- Azure Monitor

In this context, we use KQL in **Azure Resource Graph** to query Azure resources across subscriptions.

---

## 🧱 Basic Structure of a KQL Query

A KQL query is built as a pipeline of operations:

```kusto
Resources
| where type == "microsoft.compute/virtualmachines"
| project name, location
```

### Key Concepts:

- **Resources** → Main table (all Azure resources)
- **| (pipe)** → Pass results to the next operation
- **where** → Filter data
- **project** → Select columns
- **extend** → Create calculated fields
- **summarize** → Aggregate data
- **mv-expand** → Expand arrays

---

## 🎯 Goal of These Queries

These queries are designed to:

- Analyze **security exposure** (public IPs)
- Understand **infrastructure inventory**
- Detect **cost optimization opportunities**
- Identify **architecture dependencies** (paired regions, DR)
- Support **Landing Zone governance decisions**

---

# 🔐 Public IP Security Audit - Azure Resource Graph Queries

## 1. Public IP Inventory

Lists all Public IP resources.

👉 Purpose:
- Identify internet-facing endpoints
- Starting point for exposure analysis

```kusto
Resources
| where type == 'microsoft.network/publicipaddresses'
| project name, location, sku.name, properties.ipAddress
```

---

## 2. Public IPs Attached to Virtual Machines

Finds public IPs directly linked to VM NICs.

👉 Risk:
- Direct inbound access to compute

```kusto
Resources
| where type == 'microsoft.network/networkinterfaces'
| mv-expand ipconfig = properties.ipConfigurations
| where isnotempty(ipconfig.properties.publicIPAddress)
| project vm = properties.virtualMachine.id, publicIp = ipconfig.properties.publicIPAddress.id
```

---

## 3. Public IPs with NSG Protection

Checks if NICs have NSG protection.

👉 Purpose:
- Validate security controls

```kusto
Resources
| where type == 'microsoft.network/networkinterfaces'
| project nicId = id, nsg = properties.networkSecurityGroup.id, vm = properties.virtualMachine.id
```

---

## 4. Internet-facing VMs without NSGs

Detects VMs without NSG protection.

👉 High-risk scenario

```kusto
Resources
| where type == 'microsoft.compute/virtualmachines'
| where isempty(properties.networkProfile.networkInterfaces[*].properties.networkSecurityGroup)
```

---

## 5. Public IP SKU (Basic vs Standard)

Counts Public IP types.

👉 Purpose:
- Identify insecure legacy SKUs

```kusto
Resources
| where type == 'microsoft.network/publicipaddresses'
| summarize count() by tostring(sku.name)
```

---

# 💰 Licensing & Cost Optimization (Azure Hybrid Benefit)

## 1. Windows Virtual Machines (Azure Hybrid Benefit)

### 📖 Official Documentation
https://learn.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing

### 🔍 Kusto Query

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend license = tostring(properties.licenseType)
| where license == "Windows_Server"
| project name, resourceGroup, subscriptionId, license
```

### 🧠 Explanation

Azure Hybrid Benefit (AHB) for Windows VMs allows you to reuse on-premises Windows Server licenses.

👉 To have AHB **enabled**, the field must be:

```text
licenseType = "Windows_Server"
```

### ⚠️ Important Notes

- If `licenseType` is empty → AHB is **NOT enabled**
- This applies only to Windows VMs
- Helps reduce compute costs significantly

---

## 2. Azure SQL (Azure Hybrid Benefit)

### 📖 Official Documentation
https://learn.microsoft.com/en-us/azure/azure-sql/azure-hybrid-benefit?view=azuresql&tabs=azure-portal

### 🔍 Kusto Query

```kusto
resources
| where type in ("microsoft.sql/servers/databases", "microsoft.sql/managedinstances")
| extend licenseType = tostring(properties.licenseType)
| project name, type, resourceGroup, subscriptionId, licenseType
```

### 🧠 Explanation

Azure Hybrid Benefit for SQL allows reuse of SQL Server licenses.

👉 To have AHB **enabled**, the field must be:

```text
licenseType = "BasePrice"
```

### ⚠️ Important Notes

- `BasePrice` means you are bringing your own license (AHB enabled)
- If empty → full Azure license cost applies
- Applies to:
  - Azure SQL Databases
  - Managed Instances

---

# 🧠 Summary

| Service        | Field           | Value Required     | Meaning                     |
|---------------|----------------|------------------|-----------------------------|
| Windows VM     | licenseType    | Windows_Server   | AHB enabled                 |
| Azure SQL      | licenseType    | BasePrice        | AHB enabled                 |

---

# 🎯 Why This Matters

- Identifies **cost optimization opportunities**
- Detects **misconfigured licensing**
- Supports **FinOps and governance strategies**

---

# 🖥️ VM Inventory & Configuration

## 8. OS Distribution

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend osType = tostring(properties.storageProfile.osDisk.osType)
| summarize count() by osType
```

👉 Purpose:
- Understand workload distribution

---

## 9. Detailed VM Inventory

```kusto
resources
| where type == "microsoft.compute/virtualmachines"
| extend osType = tostring(properties.storageProfile.osDisk.osType)
| extend vmSize = tostring(properties.hardwareProfile.vmSize)
| extend location = tostring(location)
| extend osDiskName = tostring(properties.storageProfile.osDisk.name)
| extend osDiskSizeGB = tostring(properties.storageProfile.osDisk.diskSizeGb)
| extend osDiskType = tostring(properties.storageProfile.osDisk.managedDisk.storageAccountType)
| extend dataDiskCount = array_length(properties.storageProfile.dataDisks)
| extend licenseType = tostring(properties.licenseType)
| extend powerState = tostring(properties.extended.instanceView.powerState.displayStatus)
| extend provisioningState = tostring(properties.provisioningState)
| project 
    name,
    resourceGroup,
    subscriptionId,
    location,
    osType,
    vmSize,
    osDiskName,
    osDiskSizeGB,
    osDiskType,
    dataDiskCount,
    licenseType,
    powerState,
    provisioningState
```

👉 Purpose:
- Full VM inventory for operations and governance

---

# 🔥 Firewall Inventory

```kusto
resources
| where type == "microsoft.network/azurefirewalls"
| project
    name,
    resourceGroup,
    location,
    skuName = tostring(properties.sku.name),
    skuTier = tostring(properties.sku.tier)
```

👉 Purpose:
- Identify deployed Azure Firewalls and SKUs

---

# 🌍 Paired Regions & DR Dependencies

## 10. Storage with GRS / RA-GRS

```kusto
Resources
| where type =~ "microsoft.storage/storageaccounts"
| project name, resourceGroup, location, sku = sku.name
| where sku contains "GRS"
```

👉 Purpose:
- Detect dependency on paired regions

---

## 11. SQL Geo-Replication

```kusto
Resources
| where type =~ "microsoft.sql/servers/databases"
| extend geo = properties.secondaryType
| where isnotempty(geo)
| project name, resourceGroup, location, geo
```

👉 Purpose:
- Identify cross-region DR setups

---

## 12. Key Vault Redundancy

```kusto
Resources
| where type =~ "microsoft.keyvault/vaults"
| project name, location, sku = properties.sku.name
```

👉 Note:
- Geo-redundancy is implicit

---

# 🌐 Networking Inventory

```kusto
Resources
| where type startswith "microsoft.network"
| where type in~ (
    "microsoft.network/virtualnetworkgateways",
    "microsoft.network/connections",
    "microsoft.network/expressroutecircuits",
    "microsoft.network/expressroutegateways",
    "microsoft.network/vpngateways",
    "microsoft.network/vpnsites",
    "microsoft.network/virtualwans",
    "microsoft.network/virtualhubs"
)
| project name, type, location, resourceGroup, subscriptionId
| order by ['location'] asc
```

👉 Purpose:
- Inventory of connectivity components

---

# 📍 Regional Distribution

## 13. Resource Count per Region

```kusto
Resources
| where location != ""
| summarize count() by location
| order by location asc
```

---

## 14. Global Resources

```kusto
Resources
| where location in~ ("global")
| project location, name, type, resourceGroup, subscriptionId
```

---

## 15. Clean Region Count (Exclude noise)

```kusto
Resources
| where location != ""
| where type !in~ (
    "microsoft.network/networkwatchers"
)
| summarize count() by location
```

---

# 🔐 Key Vault CLI (Complementary)

```bash
az keyvault list -o table
```

👉 Purpose:
- Quick CLI inventory outside ARG

---

# ⚠️ Notes & Limitations

- ARG provides **inventory-level visibility**, not runtime behavior
- Does NOT show:
  - actual traffic
  - firewall inspection
  - effective NSG rules at subnet level
- Should be combined with:
  - Azure Monitor
  - Network Watcher
  - Defender for Cloud

---

# 🎯 Final Summary

These queries allow you to:

- Detect **internet exposure risks**
- Understand **infrastructure distribution**
- Identify **cost optimization opportunities**
- Reveal **hidden dependencies (paired regions)**
- Support **Landing Zone design decisions**

---
