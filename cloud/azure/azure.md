
## Az-104

1. [Command Line Interfaces](azure-Command-Line-Interfaces-104.md)
2. [Azure Storage](azure-storage-az104.md)
3. [Azure LoadBalancer](azure-loadbalancer-az104.md)
4. [Azure DNS](azure-dns-az104.md)
5. [Azure DNS Zones](azure-dns-zones-az104.md)
6. [Azure VPN](azure-vpn-az104.md)
7. [Azure File Sync](azure-filesync-az104.md)
8. [Azure EntraId](azure-entraid-104.md)
8. [Azure Identidades EntraId](azure-identidades-entra.md)
9. [Azure Storage](azure-storage-104.md)
10. [Azure Hub & Spoke](azure-hub-and-spoke-104.md)
11. [Azure Policy](azure-policy-104.md)
12. [Azure Move Resources](azure-move-res-104.md)
13. [Azure Resoruce Graph](azure-resoruce-graph-104.md)
14. [Azure Biceps](azure-biceps-104.md)
15. [Azure Kusto ](azure-kusto-104.md)
16. [Azure AppService](Azure-AppService-az104.md)
17. [Azure RBAC Conditions](Azure-RBAC-Conditions-az104.md)
18. [Azure RBAC](azure-rbac-az104.md)
19. [Azure Blob](azure-blob-az104.md)
20. [Azure Object Replication](azure-object-replication-az104.md)
21. [Azure Container](azure-container-az104.md)
22. [Azure VMs](azure-Vms-az104.md)
23. [Azure CLI](azure-cli-az104.md)
24. [Azure Backup](azure-backup-az104.md)
25. [Azure NVA](azure-nva-az104.md)
26. [Azure Identity](azure-identity-az104.md)
27. [Azure Vault](azure-vault-az104.md)
28. [Azure Network](azure-network-az104.md)
29. [Azure Disk Encryption](azure-disk-encryption-az104.md)
30. [Azure Certificates](azure-certificates-az104.md)
31. [Azure Monitor Data Collection](azure-monitor-data-collection-az104.md)
31. [Azure Monitor Alerts](azure-monitor-alerts-az104.md)
32. [Azure Storage Encryption Scopes](azure-storage-encription-scopes-az104.md)
33. [Azure ITSM Connector](azure-ITSM-connector-az104.md)
34. [Azure Subnetting](azure-subnetting-az194.md)
35. [Azure Availability Sets](azure-availability-sets-az-104.md)
36. [Azure Usuarios y Licencias](azure-usuarios-licencias-az104.md)
37. [Azure Network Security Group](azure-nsg-az104.md)
38. [Azure WAF](azure-waf.md)
39. [Azure DDOS](azure-ddos-pp-az104.md)
40. [Azure Application Gateway](azure-application-gateway-az104.md)
41. [Azure Flow Logs](azure-flow-logs-az104.md)
42. [Azure VNet Flow Logs](azure-vnet-flow-logs-az104.md)
42. [Azure VNet Flow Logs - CrowdStrike](azure-vnetflowlogs-siem.md)
43. [Azure Network Watcher](azure-NetworkWatcher.md)
44. [Azure Private Endpoint](azure-private-endpoint.md)
45. [Azure ARM](azure-arm-az104.md)
46. [Azure FIREWALL](azure-firewall.md)
47. [Azure PIM](azure-pim.md)
48. [Azure Log Analytics Workspace](azure-log-analytics-workspace.md)
49. [Azure Network Watcher](azure-network-watcher.md)
50. [Azure AMBA](azure-amba.md)
51. [Azure azure Defender Cloud ](azure-defender-for-cloud.md)
52. [Azure Resource Manager](azure-razure-resource-manager-az104.md)
56. [Azure Bastion](azure-bastion.md)
57. [Azure VPN](azure-vpn.md)
58. [Azure Powershell](azure-powershell-az104.md)
59. [Azure ResourceGroup](azure-rg-az104.md)
60. [Preguntes AZ104](azure-resum-preguntes.md)
100. [Azure vWAN](azure-vwan.md)
101. [Azure 4 Planes](azure-4-planes.md)

## Az-305

1. [Azure Client](azure-client.md)
2. [Azure Concepts](azure-concepts.md)
3. [Azure SQL Database](azure-sql-database.md)
4. [Azure Kubernetes](azure-kubernetes.md)
5. [Azure LoadBalancer](azure-loadbalancer.md)
6. [Azure Services](azure-services.md)
7. [Azure Data Migration](azure-data-migration.md)
8. [Azure EntraID](azure-entra.md)
9. [Azure Networking](azure-networking.md)
10. [Azure Cosmosdb](azure-cosmosdb.md)
11. [Azure Virtual Machines](azure-virtual-machines.md)
12. [Azure Storage](azure-storage.md)
13. [Azure Site Recovery](azure-site-recovery.md)
14. [Azure Mysql](azure-mysql.md)
15. [Azure Functions](azure-functions.md)
16. [Azure Policy](azure-policy.md)
16. [Azure Policy Assignments](azure-policy-assignments.md)
16. [Azure Policy ALZ](azure-policy-alz.md)
17. [Azure App Services](azure-app-services.md)
18. [Azure Logic Apps](azure-logic-apps.md)
19. [Azure Key Vault](azure-key-vault.md)
20. [Azure Hybrid Benefit](azure-hybrid-benefit.md)
21. [Azure Recovery Services](azure-reovery-services.md)
22. [Azure Backup](azure-backup.md)
23. [Azure Disaster Recovery](azure-disaster-recovery.md)
24. [Azure CIDR](azure-cidr.md)
25. [Azure IAM](azure-iam.md)
26. [Azure Roles](azure-roles.md)
27. [Azure Policy Assigments](azure-policy-assignments.md)

## Az-400

1. [AZ-400: Designing and Implementing Microsoft DevOps Solutions](azure-400.md)
2. [Azure Terraform](azure-terraform.md)
3. [Azure DevOps](azure-devops.md)
4. [Azure DevOps Flow](azure-devops-flow.md)


# Siglas / Conceptos

| Sigla / Concepto            | Definición completa                                           | Qué significa en una frase                                                                                         | Ejemplo |
| --------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------- |
| **RPO**                     | Recovery Point Objective                                      | Cuánta pérdida de datos es aceptable tras un incidente.                                                            | Una base de datos con RPO de 5 minutos implica que como máximo se pueden perder 5 minutos de datos. |
| **RTO**                     | Recovery Time Objective                                       | Cuánto tiempo puede estar el sistema caído antes de recuperarse.                                                   | Un sistema con RTO de 1 hora debe restaurarse completamente en menos de 60 minutos tras una caída. |
| **PITR**                    | Point-In-Time Restore                                         | Permite restaurar una base de datos a un momento exacto anterior.                                                  | Restaurar una Azure SQL Database al estado que tenía ayer a las 14:32. |
| **LTR**                     | Long-Term Retention                                           | Permite conservar backups durante años para cumplimiento normativo.                                                | Guardar backups de una base de datos durante 7 años por requisitos legales. |
| **DTU**                     | Database Transaction Unit                                     | Métrica combinada de CPU, memoria y IO usada como modelo de rendimiento en Azure SQL.                             | Una base de datos configurada con 100 DTU tiene una capacidad fija de recursos de cálculo y IO. |
| **SKU**                     | Stock Keeping Unit                                            | Identificador comercial que define una combinación concreta de características, capacidad y precio de un servicio.| Azure VM SKU **Standard_D4s_v5** define tamaño, CPU, RAM y precio. |
| **Availability Set**        | Conjunto lógico de VMs distribuidas en Fault y Update Domains | Garantiza que varias VMs no fallen a la vez dentro de la misma región.                                             | Dos VMs de una aplicación web colocadas en el mismo Availability Set. |
| **Availability Group (AG)** | Always On Availability Group                                  | Tecnología de SQL Server que replica bases entre instancias con failover automático.                              | Un cluster SQL Server con réplica primaria y secundaria sincronizada. |
| **Fault Domain (FD)**       | Dominio de fallo físico                                       | Grupo de recursos que comparten hardware; si ese hardware falla, todo el dominio cae.                             | Dos VMs en distintos Fault Domains para evitar que un fallo de rack afecte a ambas. |
| **Update Domain (UD)**      | Dominio de actualización                                      | Grupo de recursos que se actualizan al mismo tiempo durante mantenimiento planificado.                            | Azure reinicia primero las VMs del UD1 y después las del UD2 durante mantenimiento. |
| **Failover Group**          | Grupo de conmutación por error entre regiones                 | Permite replicar bases entre regiones y hacer failover automático con endpoint único.                             | Azure SQL replicada entre West Europe y North Europe con failover automático. |
| **SAML**                    | Security Assertion Markup Language                            | Estándar XML para autenticación federada entre proveedor de identidad y aplicación. Stateful, más pesado, común en apps legacy/corporativas. | Login en una aplicación corporativa usando SAML contra Entra ID. |
| **JWT**                     | JSON Web Token                                                | Token compacto en formato JSON usado para autenticación y autorización basada en claims. Stateless, ligero, ideal para APIs y microservicios. | Un API recibe un JWT emitido por Entra ID para autorizar llamadas. |
| **NVA**                     | Network Virtual Appliance                                     | Dispositivo de red tradicional (firewall, router, IDS…) ejecutado como máquina virtual dentro de Azure.           | Un firewall **Palo Alto VM-Series** desplegado en una VNet de Azure. |
| **Workload**                | Conjunto de recursos que soportan una aplicación o servicio   | Agrupación lógica de recursos (VMs, DB, red, storage) que trabajan juntos para ofrecer una funcionalidad.         | Una aplicación web compuesta por App Service, Azure SQL y Storage. |
| **Service Group**           | Agrupación lógica de servicios relacionados                   | Conjunto organizado de servicios que cumplen una función común dentro de una arquitectura o plataforma.           | Grupo de servicios que forman una plataforma de analítica de datos. |
| **Management Group**        | Contenedor jerárquico para gobernanza en Azure                | Nivel superior que permite organizar suscripciones y aplicar políticas y RBAC de forma centralizada.              | Management Group **Production** que contiene varias suscripciones de producción. |
| **Tenant**                  | Instancia dedicada de **Microsoft Entra ID (antes Azure Active Directory)** que representa a una organización en la nube y donde se gestionan identidades, dominios y acceso a servicios. | Es el **espacio de identidad de una organización** en Azure: todos los usuarios, aplicaciones y suscripciones pertenecen a un tenant. | Un tenant **company.com** que gestiona usuarios como **peter@company.com**, aplicaciones y acceso a Azure. Si no se define dominio propio se crea uno como **company.onmicrosoft.com**. |
| **Subscription**            | Contenedor de facturación y gestión de recursos en Azure      | Unidad que agrupa recursos de Azure y define cómo se facturan los servicios utilizados.                           | Subscription **Company-Production-Platform** que contiene recursos de producción. |
| **Account (Azure Account)** | Identidad en Entra ID que puede ser un usuario o una aplicación y que se usa para autenticarse y acceder a recursos en Azure | Representa quién o qué se autentica en Azure para realizar operaciones o acceder a recursos.                     | Usuario **peter@company.com** accediendo al portal de Azure o una aplicación autenticándose mediante un **Service Principal** para desplegar recursos automáticamente. |
| **Resource Group**          | Contenedor lógico para agrupar recursos de Azure relacionados | Permite organizar y gestionar recursos que pertenecen a la misma aplicación o ciclo de vida.                     | Resource Group **rg-prod-webapp** que contiene App Service, Storage y Key Vault. |
| **Break Glass accounts**    | are **emergency administrator accounts** used to access a system when normal authentication methods fail. | They are designed to be used **only in critical situations** where administrators cannot access the environment through normal accounts. | Una cuenta Global Admin guardada offline para acceder al tenant en caso de emergencia. |
| **Claim**                   | Información sobre una identidad incluida en un token de autenticación | OAuth / OpenID Connect / Entra ID                                                                                  | Un claim **email=peter@company.com** dentro de un JWT emitido por Entra ID. |
| **Access Key (Storage)**    | Una access key es una **clave secreta compartida** que da acceso completo a la cuenta de storage | Autenticación basada en claves                                                                                     | Usar la **Storage Account Key** para acceder a blobs mediante una aplicación. |
