
## Public IP Inventory

### 1. Public IP Inventory
Lists all Public IP resources in the subscription. This gives a complete overview of which workloads are potentially internet-facing and is the starting point for any exposure analysis.
[Consultar la consulta en la referencia de Resource Graph](azure-kusto-104.md#1-public-ip-inventory)
### 2. Public IPs Attached to Virtual Machines
Identifies public IPs that are directly associated with virtual machine network interfaces. These are typically the most sensitive exposures, as they allow direct inbound connectivity to compute workloads.
[Consultar la consulta en la referencia de Resource Graph](azure-kusto-104.md#2-public-ips-attached-to-virtual-machines)
### 3. Public IPs with NSG Protection
Shows whether a public-facing VM is protected by a Network Security Group (NSG). NSGs are a core security control for limiting inbound access from the internet.
[Consultar la consulta en la referencia de Resource Graph](azure-kusto-104.md#3-public-ips-with-nsg-protection)
### 4. Internet-facing VMs without NSGs
Highlights virtual machines that have a public IP but no effective NSG protection. These represent an increased security risk and should be reviewed with priority.
[Consultar la consulta en la referencia de Resource Graph](azure-kusto-104.md#4-internet-facing-vms-without-nsgs)
### 5. Public IP SKU (Basic vs Standard)
Distinguish between Basic and Standard Public IP SKUs. Standard SKUs are recommended as they are secure by default and integrate better with modern Azure networking and security features.
[Consultar la consulta en la referencia de Resource Graph](azure-kusto-104.md#5-public-ip-sku-basic-vs-standard)
