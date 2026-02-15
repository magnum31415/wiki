[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Site Recovery 

Azure Site Recovery helps ensure business continuity by keeping business apps and workloads running during outages.
Site Recovery replicates workloads running on physical and virtual machines (VMs) from a primary site to a secondary location.
When an outage occurs at your primary site, you fail over to a secondary location and access apps from there. 
After the primary location is running again, you can fail back to it.

Site Recovery can manage replication for:
 - Azure VMs replicating between Azure regions
 - Replication from Azure Public Multi-Access Edge Compute (MEC) to the region
 - Replication between two Azure Public MECs
 - On-premises VMs, Azure Stack VMs, and physical servers

![azure-site-recovery-sql-vm](./img/azure/azure-site-recovery-sql-vm.png)


## Virtual Machines:

Azure Site Recovery ensures continuous replication to a secondary Azure region, facilitating rapid service restoration in an alternate region during primary region outages.
This capability is critical for meeting the rapid recovery requirement.

## Azure SQL Database:

Active Geo-Replication offers real-time data replication to a secondary region, which is crucial for data protection.
Auto-failover groups enhance this by automating the failover process, which is essential for achieving rapid recovery with minimal manual intervention.

## Blob Storage:

GRS provides data replication to a secondary geographic location, ensuring data is protected against regional outages. 
RA-GRS adds the benefit of read access to the replicated data in the secondary region, ensuring data availability even if the primary region is compromised. Both features satisfy the requirement of storing the unstructured data in another region.

![geo replication](./img/azure/azure-sql-database-geo-replication.png)
