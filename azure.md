
<table>
  <tr>
    <th>Az-104</th>
    <th>Az-305</th>
  </tr>
  <tr>
    <td>
      1.<a href="https://github.com/magnum31415/wiki/blob/main/azure-Command-Line-Interfaces-104.md">Command Line Interfaces</a><br>
      2.<a href="https://github.com/magnum31415/wiki/blob/main/azure-storage-az104.md">Azure Storage</a><br>
      3.<a href="https://github.com/magnum31415/wiki/blob/main/azure-loadbalancer-az104.md">Azure LoadBalancer</a><br>
      4.<a href="https://github.com/magnum31415/wiki/blob/main/azure-dns-az104.md">Azure DNS</a><br>
      5.<a href="https://github.com/magnum31415/wiki/blob/main/azure-vpn-az104.md">Azure VPN</a><br>
      6.<a href="https://github.com/magnum31415/wiki/blob/main/azure-filesync-az104.md">Azure File Sync</a><br>
      7.<a href="https://github.com/magnum31415/wiki/blob/main/azure-entraid-104.md">Azure EntraId</a><br>
      8.<a href="https://github.com/magnum31415/wiki/blob/main/azure-storage-104.md">Azure Storage</a><br>
      9.<a href="https://github.com/magnum31415/wiki/blob/main/azure-hub-and-spoke-104.md">Azure Hub & Spoke</a><br>
    </td>
    <td>
      1. <a href="https://github.com/magnum31415/wiki/blob/main/azure-client.md">Azure Client</a><br>
      2. <a href="https://github.com/magnum31415/wiki/blob/main/azure-concepts.md">Azure Concepts</a><br>
      3. <a href="https://github.com/magnum31415/wiki/blob/main/azure-sql-database.md">Azure SQL Database</a><br>
      4. <a href="https://github.com/magnum31415/wiki/blob/main/azure-kubernetes.md">Azure Kubernetes</a><br>
      5. <a href="https://github.com/magnum31415/wiki/blob/main/azure-loadbalancer.md">Azure LoadBalancer</a><br>
      6. <a href="https://github.com/magnum31415/wiki/blob/main/azure-services.md">Azure Services</a><br>
      7. <a href="https://github.com/magnum31415/wiki/blob/main/azure-data-migration.md">Azure Data Migration</a><br>
      8. <a href="https://github.com/magnum31415/wiki/blob/main/azure-entra.md">Azure EntraID</a><br>
      9. <a href="https://github.com/magnum31415/wiki/blob/main/azure-networking.md">Azure Networking</a><br>
      10. <a href="https://github.com/magnum31415/wiki/blob/main/azure-cosmosdb.md">Azure Cosmosdb</a><br>
      11. <a href="https://github.com/magnum31415/wiki/blob/main/azure-virtual-machines.md">Azure Virtual Machines</a><br>
      12. <a href="https://github.com/magnum31415/wiki/blob/main/azure-storage.md">Azure Storage</a><br>
      13. <a href="https://github.com/magnum31415/wiki/blob/main/azure-site-recovery.md">Azure Site Recovery</a><br>
      14. <a href="https://github.com/magnum31415/wiki/blob/main/azure-mysql.md">Azure Mysql</a><br>
      15. <a href="https://github.com/magnum31415/wiki/blob/main/azure-functions.md">Azure Functions</a><br>
      16. <a href="https://github.com/magnum31415/wiki/blob/main/azure-policy.md">Azure Policy</a><br>
      17. <a href="https://github.com/magnum31415/wiki/blob/main/azure-app-services.md">Azure App Services</a><br>
      18. <a href="https://github.com/magnum31415/wiki/blob/main/azure-logic-apps.md">Azure Logic Apps</a><br>
      19. <a href="https://github.com/magnum31415/wiki/blob/main/azure-key-vault.md">Azure Key Vault</a><br>
      20. <a href="https://github.com/magnum31415/wiki/blob/main/azure-hybrid-benefit.md">Azure Hybrid Benefit</a><br>
      21. <a href="https://github.com/magnum31415/wiki/blob/main/azure-reovery-services.md">Azure Recovery Services</a><br>
      22. <a href="https://github.com/magnum31415/wiki/blob/main/azure-backup.md">Azure Backup</a><br>
      23. <a href="https://github.com/magnum31415/wiki/blob/main/azure-disaster-recovery.md">Azure Disaster Recovery</a><br>
      24. <a href="https://github.com/magnum31415/wiki/blob/main/azure-cidr.md">Azure CIDR</a><br>
      25. <a href="https://github.com/magnum31415/wiki/blob/main/azure-iam.md">Azure IAM</a>
    </td>

  </tr>
</table>


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
| **Tenant**                  | Instancia dedicada de Microsoft Entra ID que representa a una organización en la nube | Espacio aislado donde se gestionan identidades, usuarios, dominios y acceso a servicios como Azure o Microsoft 365. | El tenant **synlab.com** donde se gestionan usuarios y grupos corporativos. |
| **Subscription**            | Contenedor de facturación y gestión de recursos en Azure      | Unidad que agrupa recursos de Azure y define cómo se facturan los servicios utilizados.                           | Subscription **Synlab-Production-Platform** que contiene recursos de producción. |
| **Account (Azure Account)** | Identidad usada para crear y administrar suscripciones en Azure | Usuario o identidad que tiene acceso al tenant y puede administrar recursos según sus permisos.                   | Un usuario **peter@company.com** con rol Owner en una suscripción. |
| **Resource Group**          | Contenedor lógico para agrupar recursos de Azure relacionados | Permite organizar y gestionar recursos que pertenecen a la misma aplicación o ciclo de vida.                     | Resource Group **rg-prod-webapp** que contiene App Service, Storage y Key Vault. |
| **Break Glass accounts**    | are **emergency administrator accounts** used to access a system when normal authentication methods fail. | They are designed to be used **only in critical situations** where administrators cannot access the environment through normal accounts. | Una cuenta global admin almacenada offline para emergencias en Entra ID. |
| **Claim**                   | Información sobre una identidad incluida en un token de autenticación | OAuth / OpenID Connect / Entra ID                                                                                  | Un claim **email=peter@company.com** dentro de un JWT. |
| **Access Key (Storage)**    | Una access key es una **clave secreta compartida** que da acceso completo a la cuenta de storage | Autenticación basada en claves                                                                                     | Usar la **Storage Account Key** para acceder a blobs mediante una aplicación. |
