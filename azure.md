
1. [Azure Client](https://github.com/magnum31415/wiki/blob/main/azure-client.md)
2. [Azure Concepts](https://github.com/magnum31415/wiki/blob/main/azure-concepts.md)
3. [Azure SQL Database](https://github.com/magnum31415/wiki/blob/main/azure-sql-database.md)
4. [Azure Kubernetes](https://github.com/magnum31415/wiki/blob/main/azure-kubernetes.md)
5. [Azure LoadBalancer](https://github.com/magnum31415/wiki/blob/main/azure-loadbalancer.md)
6. [Azure Services](https://github.com/magnum31415/wiki/blob/main/azure-services.md)
7. [Azure Data Migration](https://github.com/magnum31415/wiki/blob/main/azure-data-migration.md)
8. [Azure Entra](https://github.com/magnum31415/wiki/blob/main/azure-entra.md)
9. [Azure Networking](https://github.com/magnum31415/wiki/blob/main/azure-networking.md)
10. [Azure Cosmosdb](https://github.com/magnum31415/wiki/blob/main/azure-cosmosdb.md)
11. [Azure Virtual Machines](https://github.com/magnum31415/wiki/blob/main/azure-virtual-machines.md)
12. [Azure storage](https://github.com/magnum31415/wiki/blob/main/azure-storage.md)
13. [Azure Site Recovery](https://github.com/magnum31415/wiki/blob/main/azure-site-recovery.md)
14. [Azure Mysql](https://github.com/magnum31415/wiki/blob/main/azure-mysql.md)
15. [Azure Functions](https://github.com/magnum31415/wiki/blob/main/azure-functions.md)
16. [Azure Policy](https://github.com/magnum31415/wiki/blob/main/azure-policy.md)
17. [Azure App Services](https://github.com/magnum31415/wiki/blob/main/azure-app-services.md)
18. [Azure Logic Apps](https://github.com/magnum31415/wiki/blob/main/azure-logic-apps.md)
19. [Azure Key Vault](https://github.com/magnum31415/wiki/blob/main/azure-key-vault.md)
20. [Azure Hybrid Benefit](https://github.com/magnum31415/wiki/blob/main/azure-hybrid-benefit.md)
21. [Azure Recovery Services](https://github.com/magnum31415/wiki/blob/main/azure-reovery-services.md)
22. [Azure Backup](https://github.com/magnum31415/wiki/blob/main/azure-backup.md)
23. [Azure Disaster Recovery](https://github.com/magnum31415/wiki/blob/main/azure-disaster-recovery.md)
24. [Azure CIDR](https://github.com/magnum31415/wiki/blob/main/azure-cidr.md)
24. [Azure IAM](https://github.com/magnum31415/wiki/blob/main/azure-iam.md)



# Siglas

| Sigla / Concepto            | Definición completa                                           | Qué significa en una frase                                                                                         |
| --------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **RPO**                     | Recovery Point Objective                                      | Cuánta pérdida de datos es aceptable tras un incidente.                                                            |
| **RTO**                     | Recovery Time Objective                                       | Cuánto tiempo puede estar el sistema caído antes de recuperarse.                                                   |
| **PITR**                    | Point-In-Time Restore                                         | Permite restaurar una base de datos a un momento exacto anterior.                                                  |
| **LTR**                     | Long-Term Retention                                           | Permite conservar backups durante años para cumplimiento normativo.                                                |
| **DTU**                     | Database Transaction Unit                                     | Métrica combinada de CPU, memoria y IO usada como modelo de rendimiento en Azure SQL.                              |
| **SKU**                     | Stock Keeping Unit                                            | Identificador comercial que define una combinación concreta de características, capacidad y precio de un servicio. |
| **Availability Set**        | Conjunto lógico de VMs distribuidas en Fault y Update Domains | Garantiza que varias VMs no fallen a la vez dentro de la misma región.                                             |
| **Availability Group (AG)** | Always On Availability Group                                  | Tecnología de SQL Server que replica bases entre instancias con failover automático.                               |
| **Fault Domain (FD)**       | Dominio de fallo físico                                       | Grupo de recursos que comparten hardware; si ese hardware falla, todo el dominio cae.                              |
| **Update Domain (UD)**      | Dominio de actualización                                      | Grupo de recursos que se actualizan al mismo tiempo durante mantenimiento planificado.                             |
| **Failover Group**          | Grupo de conmutación por error entre regiones                 | Permite replicar bases entre regiones y hacer failover automático con endpoint único.                              |
| **SAML**                    | Security Assertion Markup Language                            | Estándar XML para autenticación federada entre proveedor de identidad y aplicación. Basado en XML para SSO entre sistemas. Es sataetful, pesado , apps legacy /corporativas                               |
| **JWT**                     | JSON Web Token                                                | Token compacto en formato JSON usado para autenticación y autorización basada en claims. Token ligero y firmado que contiene los claims del usaurio. Es stateless,  ligero/rápido, apis/,microservicos/cloud                           |
|**NVA** | Network Virtual Appliance | Un dispositivo de red tradicional (firewall, router, IDS…) pero en versión máquina virtual dentro de Azure.|
