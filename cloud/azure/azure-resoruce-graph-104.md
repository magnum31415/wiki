
## Public IP Inventory

### 1. Public IP Inventory
Lists all Public IP resources in the subscription. This gives a complete overview of which workloads are potentially internet-facing and is the starting point for any exposure analysis.
````
Resources | where type == 'microsoft.network/publicipaddresses' | project name, location, sku.name, properties.ipAddress
````
### 2. Public IPs Attached to Virtual Machines
Identifies public IPs that are directly associated with virtual machine network interfaces. These are typically the most sensitive exposures, as they allow direct inbound connectivity to compute workloads.
````
Resources | where type == 'microsoft.network/networkinterfaces' | mv-expand ipconfig = properties.ipConfigurations | where isnotempty(ipconfig.properties.publicIPAddress) | project vm = properties.virtualMachine.id, publicIp = ipconfig.properties.publicIPAddress.id
````
### 3. Public IPs with NSG Protection
Shows whether a public-facing VM is protected by a Network Security Group (NSG). NSGs are a core security control for limiting inbound access from the internet.
````
Resources | where type == 'microsoft.network/networkinterfaces' | project nicId = id, nsg = properties.networkSecurityGroup.id, vm = properties.virtualMachine.id
````
### 4. Internet-facing VMs without NSGs
Highlights virtual machines that have a public IP but no effective NSG protection. These represent an increased security risk and should be reviewed with priority.
````
Resources | where type == 'microsoft.compute/virtualmachines' | where isempty(properties.networkProfile.networkInterfaces[*].properties.networkSecurityGroup)
````
### 5. Public IP SKU (Basic vs Standard)
Distinguish between Basic and Standard Public IP SKUs. Standard SKUs are recommended as they are secure by default and integrate better with modern Azure networking and security features.
````
Resources | where type == 'microsoft.network/publicipaddresses' | summarize count() by tostring(sku.name)
````
