[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Site Recovery 

Es un servicio de Disaster Recovery (DR) que replica máquinas (VMs o físicas) a otra ubicación para poder arrancarlas allí si el sitio principal falla.

- 👉 No es backup.
- 👉 No es alta disponibilidad local.
  
-🔹 Es **recuperación ante desastre completo.**
-🔹 ASR es principalmente para **replicación entre regiones**.
-🔹 **No está diseñado para replicación entre Availability Zones**.

**Resumen mental rápido**

````
Azure Site Recovery =
Replica VMs → No ejecuta VMs secundarias → Arranca solo si hay desastre → Permite failback → DR completo
````

![azure-site-recovery-sql-vm](./img/azure/azure-site-recovery-sql-vm.png)


| Característica | Azure Site Recovery |
|---------------|--------------------|
| Tipo de servicio | Disaster Recovery (DR) |
| ¿Es Backup? | ❌ No |
| ¿Es Alta Disponibilidad local (HA)? | ❌ No |
| Objetivo principal | Recuperación ante desastre completo (datacenter o región) |
| Qué replica | Máquinas completas (VMs o físicas) |
| Nivel de replicación | Nivel de máquina (disco/infraestructura), no aplicación |
| Tipo de protección | Infraestructura completa |
| Regiones soportadas | Cualquier región Azure |
| Azure → Azure | ✅ Sí |
| On-prem → Azure | ✅ Sí |
| Azure Stack → Azure | ✅ Sí |
| Azure Public MEC | ✅ Sí |
| Servidores físicos | ✅ Sí |
| ¿Replica aplicaciones (SQL/IIS)? | ❌ No (replica VM completa) |
| ¿Mantiene VM secundaria encendida? | ❌ No |
| ¿Consume compute en secundario antes del failover? | ❌ No |
| ¿Dónde guarda datos replicados? | Azure Storage |
| RPO (Recovery Point Objective) | ~5 minutos (crash-consistent) |
| RPO con consistencia aplicación | ~1 hora (application-consistent) |
| RTO (Recovery Time Objective) | Normalmente < 15 minutos |
| Failover por defecto | Manual |
| Planned failover | ✅ Sí |
| Unplanned failover | ✅ Sí |
| Failover automático nativo | ❌ No |
| Automatización posible | ✅ Con Azure Automation + Recovery Plans |
| Permite failback | ✅ Sí |
| Protección ante caída de región | ✅ Sí |
| Protección ante caída de VM individual | ❌ No (eso es HA) |
| Protección ante error lógico en DB | ❌ No |
| SLA | 99.9% para el servicio |
| Coste | Bajo mientras no hay failover (solo storage + replicación) |
| Tipo de consistencia | Crash-consistent y Application-consistent |
| Orquestación de recuperación | ✅ Recovery Plans |
| Test Failover sin impacto | ✅ Sí |

---

# 🧠 Diferencia clave frente a otras soluciones

| Servicio | Qué protege | Failover automático |
|-----------|-------------|--------------------|
| Availability Zones | Fallo de zona | ✅ Sí |
| SQL Availability Group | Base de datos | ✅ Sí |
| SQL Failover Group | Base de datos | ✅ Sí |
| Azure Site Recovery | Infraestructura completa (VMs) | ❌ No (manual por defecto) |

---

# 🎯 Resumen mental rápido

Azure Site Recovery =

Replica VMs → No ejecuta VMs secundarias → Arranca solo en desastre → Permite failback → DR completo de infraestructura

**🧠 Qué problema resuelve**

Si tu datacenter o región Azure cae:
1. Se activa failover
2. Las máquinas se arrancan en la ubicación secundaria
3. Cuando el primario vuelve, puedes hacer failback

**🏗 Qué puede replicar**

ASR puede gestionar replicación de:
- ✅ Azure VM → otra región Azure
- ✅ Azure Public MEC → región
- ✅ Azure Public MEC → otro MEC
- ✅ On-prem VMs
- ✅ Azure Stack VMs
- ✅ Servidores físicos

**⚙ Cómo funciona técnicamente**
- 1️⃣ Replica sin interceptar datos de aplicación
  No se mete en SQL, IIS, etc. Replica a nivel de máquina.

- 2️⃣ Guarda datos en Azure Storage
  Mientras replica:
  - Solo almacena discos
  - No crea VMs activas
  - No consume compute (coste más bajo)

- 3️⃣ Solo crea la VM en el momento del failover
  Por eso:
  - Es más barato que tener una VM secundaria encendida.

**🌍 DR global**

Puedes replicar entre **cualquier región Azure del mundo.**

Ejemplo:
- West Europe → East US
- France Central → North Europe

**⏱ RTO y RPO**
- RTO (Recovery Time Objective)
  - Tiempo para volver a operar = 👉 Normalmente < 15 minutos.
- RPO (Recovery Point Objective)
  - 🟢 Consistencia de aplicación → ~1 hora
  - 🔵 Consistencia tipo crash → ~5 minutos

**Diferencia clave con otras soluciones**
| Servicio             | Qué protege                                     |
| -------------------- | ----------------------------------------------- |
| Availability Zones   | Fallo de zona                                   |
| Failover Group (SQL) | Caída de región (solo DB)                       |
| Azure Site Recovery  | Caída completa de infraestructura (VMs enteras) |

**¿El failover en Azure Site Recovery es automático o manual?**

- 👉 Por defecto es manual.

Pero puedes configurarlo como:
- ✅ Planned failover (migración controlada)
- ⚠️ Unplanned failover (desastre real)
- 🤖 Automático → Solo si lo integras con Azure Automation + Recovery Plans
- ASR no hace failover automático “mágico” como un Availability Zone.

**🧠 ¿Qué problema resuelve realmente?**

- Resuelve esto:
  - “Mi datacenter entero o región Azure ha caído. Necesito arrancar todo en otro sitio.”
- No protege contra:
  - Caída de una VM individual (eso es HA local)
  - Fallo de disco puntual
  - Errores lógicos en base de datos
- Protege contra:
  - 🔥 Incendio en CPD
  - 🌍 Caída regional completa
  - 🧨 Desastre mayor

**🔄 Flujo real**

1. 1️⃣ Replicas continuamente las VMs al secundario
2. 2️⃣ El primario cae
3. 3️⃣ Tú (o un plan automatizado) ejecutas failover
4. 4️⃣ Azure crea las VMs en la región secundaria
5. 5️⃣ Cuando el primario vuelve → haces failback

**Diferencia clave**

| Servicio               | Failover automático real  |
| ---------------------- | ------------------------- |
| Zone Redundancy        | ✅ Sí                      |
| SQL Availability Group | ✅ Sí                      |
| Failover Group (SQL)   | ✅ Sí                      |
| Azure Site Recovery    | ❌ No (por defecto manual) |



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
