
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

| Sigla / Concepto            | Definición completa                                           | Qué significa en una frase                                                                                         |
| --------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **RPO**                     | Recovery Point Objective                                      | Cuánta pérdida de datos es aceptable tras un incidente.                                                            |
| **RTO**                     | Recovery Time Objective                                       | Cuánto tiempo puede estar el sistema caído antes de recuperarse.                                                   |
| **PITR**                    | Point-In-Time Restore                                         | Permite restaurar una base de datos a un momento exacto anterior.                                                  |
| **LTR**                     | Long-Term Retention                                           | Permite conservar backups durante años para cumplimiento normativo.                                                |
| **DTU**                     | Database Transaction Unit                                     | Métrica combinada de CPU, memoria y IO usada como modelo de rendimiento en Azure SQL.                             |
| **SKU**                     | Stock Keeping Unit                                            | Identificador comercial que define una combinación concreta de características, capacidad y precio de un servicio.|
| **Availability Set**        | Conjunto lógico de VMs distribuidas en Fault y Update Domains | Garantiza que varias VMs no fallen a la vez dentro de la misma región.                                             |
| **Availability Group (AG)** | Always On Availability Group                                  | Tecnología de SQL Server que replica bases entre instancias con failover automático.                              |
| **Fault Domain (FD)**       | Dominio de fallo físico                                       | Grupo de recursos que comparten hardware; si ese hardware falla, todo el dominio cae.                             |
| **Update Domain (UD)**      | Dominio de actualización                                      | Grupo de recursos que se actualizan al mismo tiempo durante mantenimiento planificado.                            |
| **Failover Group**          | Grupo de conmutación por error entre regiones                 | Permite replicar bases entre regiones y hacer failover automático con endpoint único.                             |
| **SAML**                    | Security Assertion Markup Language                            | Estándar XML para autenticación federada entre proveedor de identidad y aplicación. Stateful, más pesado, común en apps legacy/corporativas. |
| **JWT**                     | JSON Web Token                                                | Token compacto en formato JSON usado para autenticación y autorización basada en claims. Stateless, ligero, ideal para APIs y microservicios. |
| **NVA**                     | Network Virtual Appliance                                     | Dispositivo de red tradicional (firewall, router, IDS…) ejecutado como máquina virtual dentro de Azure.           |
| **Workload**                | Conjunto de recursos que soportan una aplicación o servicio   | Agrupación lógica de recursos (VMs, DB, red, storage) que trabajan juntos para ofrecer una funcionalidad.         |
| **Service Group**           | Agrupación lógica de servicios relacionados                   | Conjunto organizado de servicios que cumplen una función común dentro de una arquitectura o plataforma.           |
| **Management Group**        | Contenedor jerárquico para gobernanza en Azure                | Nivel superior que permite organizar suscripciones y aplicar políticas y RBAC de forma centralizada.              |
| **Tenant**                  | Instancia dedicada de Microsoft Entra ID que representa a una organización en la nube | Espacio aislado donde se gestionan identidades, usuarios, dominios y acceso a servicios como Azure o Microsoft 365. |
| **Subscription**            | Contenedor de facturación y gestión de recursos en Azure      | Unidad que agrupa recursos de Azure y define cómo se facturan los servicios utilizados.                           |
|**Break Glass accounts**|are **emergency administrator accounts** used to access a system when normal authentication methods fail.| They are designed to be used **only in critical situations** where administrators cannot access the environment through normal accounts.|
| **Claim** | Información sobre una identidad incluida en un token de autenticación | OAuth / OpenID Connect / Entra ID|
| **Access Key (Storage)** | Una access key es una **clave secreta compartida** que da acceso completo a la cuenta de storage | Autenticación basada en claves |
