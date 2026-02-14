[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Virtual Machines

Azure Virtual Machines are cloud-based computing resources that can be used to run a wide variety of applications.
They are scalable, reliable, secure, and can be provisioned quickly and easily. 
It is also available in a variety of sizes and configurations, so you can choose the right VM for your needs.

## Disk cache

These are the recommended disk cache settings for data disks:
- **None** – configure host-cache as None for write-only and write-heavy disks.
- **ReadOnly** – configure host-cache as ReadOnly for read-only and read-write disks.
- **ReadWrite** – configure host-cache as ReadWrite only if your application properly handles writing cached data to persistent disks when needed.
