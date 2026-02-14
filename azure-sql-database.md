[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# Azure SQL Database 

Servicio PaaS basado en SQLServer totalmente gestionado 

## Funcionalitats

### Geo-replication
**Geo-replication** provides geographic redundancy and enables read operations only in the secondary region during a primary region outage.
However, it does not support write operations in the secondary region when the primary region is down.

Active geo-replication is configured per database, and only supports manual failover.

---

### Azure SQL Database Failover Group

Un Failover Group es un mecanismo de disaster recovery (DR) entre regiones para:
- Azure SQL Database (Single DB / Elastic Pool)
- Azure SQL Managed Instance
Permite agrupar una o varias bases de datos y replicarlas automáticamente a otra región de Azure.

![Diagrama arquitectura](./azure-sql-failover-group.png)

#### ¿Qué problema resuelve?

Alta disponibilidad dentro de una región ya viene incluida (HA local).

Pero si se cae una región entera (region outage), necesitas:
- Réplica en otra región
- Failover automático
- Endpoint estable que no cambie
Ahí entra Failover Group.

#### 🏗 Cómo funciona

Cuando creas un Failover Group:

##### 1️⃣ Seleccionas:

- Servidor primario (ej: West Europe)
- Servidor secundario (ej: North Europe)

##### 2️⃣ Azure crea:

- Réplica secundaria asincrónica
- Sincronización continua (geo-replication)

##### 3️⃣ Se generan 2 endpoints:
| Endpoint      | Uso                        |
| ------------- | -------------------------- |
| 🔵 Read/Write | Apunta siempre al primario |
| 🟢 Read-only  | Apunta al secundario       |


##### 🔁 Tipo de replicación

- 🌍 Geo-replication
- 🔄 Asincrónica
- RPO → segundos/minutos
- RTO → minutos
No es síncrona, por lo tanto:
- ❌ No garantiza RPO = 0
- ✅ Sí garantiza continuidad regional

##### 🚀 Ventajas clave

- Failover automático opcional
- Failover manual posible
- Endpoint DNS estable (no cambia string de conexión)
- Agrupa múltiples bases en una sola operación

---
### Availability Group (AG)

Un Availability Group (AG) es una tecnología de alta disponibilidad (HA) y opcionalmente disaster recovery (DR) de Microsoft SQL Server, basada en Windows Server Failover Clustering (WSFC).
Permite replicar una o varias bases de datos entre varias instancias de SQL Server.

#### 🧠 Idea rápida

**Availability Group = Always On + múltiples réplicas + failover automático + endpoint virtual**

#### ¿Dónde se usa?

- SQL Server on-premises
- SQL Server en Azure Virtual Machines
- Azure SQL Managed Instance (Business Critical usa una arquitectura similar internamente)
No aplica a Azure SQL Database Single DB (PaaS puro).

#### Cómo funciona

Un AG tiene:

- 🔵 1 réplica primaria (read/write)
- 🟢 1 o más réplicas secundarias (read-only opcional)
- 🎯 Un Listener (DNS virtual)

``App → AG Listener → Primaria``

- Si la primaria falla:

``→ Se promueve automáticamente una secundaria.``

**La aplicación no cambia el connection string.**

---
## Mapa Jerárquico de Azure SQL (Servicios y Modelos de Compra) - Servicios PaaS 
````graph
Azure SQL (Familia de servicios)
│
├── 1️⃣ Azure SQL Database
│   │
│   ├── A) Single Database
│   │   │
│   │   ├── Modelo de compra
│   │   │   ├── DTU
│   │   │   │   ├── Basic
│   │   │   │   ├── Standard
│   │   │   │   └── Premium
│   │   │   │
│   │   │   └── vCore
│   │   │       ├── Service Tier
│   │   │       │   ├── General Purpose
│   │   │       │   ├── Business Critical
│   │   │       │   └── Hyperscale
│   │   │       │
│   │   │       └── Compute Option
│   │   │           ├── Provisioned
│   │   │           └── Serverless (solo General Purpose)
│   │   │
│   │
│   └── B) Elastic Pool
│       │
│       ├── Modelo de compra
│       │   ├── DTU
│       │   │   ├── Basic
│       │   │   ├── Standard
│       │   │   └── Premium
│       │   │
│       │   └── vCore
│       │       ├── General Purpose
│       │       └── Business Critical
│       │
│       └── ❌ No existe Hyperscale
│       └── ❌ No existe Serverless
│
└── 2️⃣ Azure SQL Managed Instance
    │
    ├── Modelo de compra
    │   └── vCore (único modelo)
    │
    ├── Service Tier
    │   ├── General Purpose
    │   └── Business Critical
    │
    └── ❌ No existe DTU
    └── ❌ No existe Hyperscale
    └── ❌ No existe Serverless

````


## Guía de Selección de Azure SQL según Requisitos Técnicos
````yaml
¿Necesita compatibilidad casi total con SQL Server (SQL Agent, cross-DB, CLR)?
│
├── Sí → Azure SQL Managed Instance
│       │
│       ├── Modelo: Solo vCore
│       ├── Service Tier:
│       │       ├── General Purpose
│       │       └── Business Critical
│       │
│       ├── 🔎 Equivalencia conceptual:
│       │       Business Critical (MI)
│       │       ≈ Premium (DTU en Single DB)
│       │
│       ├── HA: Sí (Always On interno)
│       ├── DR: Sí (Auto-failover groups / Geo-replication)
│       ├── Read replicas: Sí (Business Critical)
│       ├── Backups: Automáticos + PITR + LTR
│       └── In-Memory OLTP: Sí (Business Critical)
│
└── No →
      ¿Carga OLTP muy alta / In-Memory / baja latencia?
      │
      ├── Sí → Azure SQL Database
      │       │
      │       ├── Modelo moderno (vCore):
      │       │       └── Business Critical
      │       │
      │       ├── Modelo antiguo (DTU):
      │       │       └── Premium
      │       │
      │       ├── 🔎 Equivalencia:
      │       │       Premium (DTU)
      │       │       ⇄ Business Critical (vCore)
      │       │
      │       ├── HA: Sí (réplicas síncronas locales)
      │       ├── DR: Sí (Auto-failover group / Geo-replication)
      │       ├── Read replicas: Sí (hasta 3)
      │       ├── Backups: Automáticos + PITR + LTR
      │       └── In-Memory OLTP: Sí
      │
      └── No →
            ¿Muchas bases con uso variable?
            │
            ├── Sí → Elastic Pool
            │       │
            │       ├── Modelo DTU:
            │       │       ├── Basic
            │       │       ├── Standard
            │       │       └── Premium
            │       │
            │       ├── Modelo vCore:
            │       │       ├── General Purpose
            │       │       └── Business Critical
            │       │
            │       ├── 🔎 Equivalencias:
            │       │       Basic (DTU)     ≈ GP bajo
            │       │       Standard (DTU)  ⇄ General Purpose (vCore)
            │       │       Premium (DTU)   ⇄ Business Critical (vCore)
            │       │
            │       ├── HA: Sí (integrado)
            │       ├── DR: Sí (Geo-replication)
            │       ├── Read replicas: Solo si Business Critical / Premium
            │       ├── Backups: Automáticos
            │       └── In-Memory OLTP: Solo en Premium / Business Critical
            │
            └── No →
                  ¿Carga intermitente / dev-test?
                  │
                  ├── Sí → Single DB – General Purpose Serverless
                  │       │
                  │       ├── Modelo: vCore (General Purpose)
                  │       ├── 🔎 Equivalencia aproximada:
                  │       │       Standard (DTU) ≈ General Purpose (vCore)
                  │       │
                  │       ├── HA: Sí (remota)
                  │       ├── DR: Sí
                  │       ├── Read replicas: No dedicadas
                  │       ├── Auto-pause: Sí
                  │       └── In-Memory OLTP: No
                  │
                  └── No → Single DB – General Purpose (Provisioned)
                          │
                          ├── Modelo vCore:
                          │       └── General Purpose
                          │
                          ├── Modelo DTU equivalente:
                          │       └── Standard
                          │
                          ├── 🔎 Equivalencia:
                          │       Standard (DTU)
                          │       ⇄ General Purpose (vCore)
                          │
                          ├── HA: Sí (réplica remota)
                          ├── DR: Sí
                          ├── Read replicas: No dedicadas
                          ├── Backups: Automáticos
                          └── In-Memory OLTP: No


````
## Tabla Completa Agrupada por Servicio

<table border="1" cellpadding="6" cellspacing="0">
<thead>
<tr>
<th rowspan="2">Característica</th>
<th colspan="3">Azure SQL Database – Single Database</th>
<th colspan="2">Azure SQL Database – Elastic Pool</th>
<th colspan="2">Azure SQL Managed Instance</th>
</tr>
<tr>
<th>General Purpose</th>
<th>Business Critical</th>
<th>Hyperscale</th>
<th>General Purpose</th>
<th>Business Critical</th>
<th>General Purpose</th>
<th>Business Critical</th>
</tr>
</thead>
<tbody>

<tr>
<td>Modelo basado en vCore</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Modelo basado en DTU disponible</td>
<td>✅</td><td>❌</td><td>❌</td>
<td>✅</td><td>❌</td>
<td>❌</td><td>❌</td>
</tr>

<tr>
<td>In-Memory OLTP</td>
<td>❌</td><td>✅</td><td>❌</td>
<td>❌</td><td>✅</td>
<td>❌</td><td>✅</td>
</tr>

<tr>
<td>Memory-Optimized Tables</td>
<td>❌</td><td>✅</td><td>❌</td>
<td>❌</td><td>✅</td>
<td>❌</td><td>✅</td>
</tr>

<tr>
<td>Columnstore Indexes</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Alta disponibilidad integrada</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Recuperación ante desastres (Geo-replication)</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Auto-failover groups</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Réplicas de solo lectura dedicadas</td>
<td>❌</td><td>✅</td><td>✅</td>
<td>❌</td><td>✅</td>
<td>❌</td><td>✅</td>
</tr>

<tr>
<td>Escalado sin downtime</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Escalado automático de computación</td>
<td>✅ (solo Serverless)</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
</tr>

<tr>
<td>Modo Serverless disponible</td>
<td>✅</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
</tr>

<tr>
<td>Auto-pause disponible</td>
<td>✅</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
</tr>

<tr>
<td>Almacenamiento local SSD</td>
<td>❌</td><td>✅</td><td>❌</td>
<td>❌</td><td>✅</td>
<td>❌</td><td>✅</td>
</tr>

<tr>
<td>Almacenamiento remoto</td>
<td>✅</td><td>❌</td><td>✅</td>
<td>✅</td><td>❌</td>
<td>✅</td><td>❌</td>
</tr>

<tr>
<td>Recursos dedicados</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>❌</td><td>❌</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>SQL Agent disponible</td>
<td>❌</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Consultas cross-database completas</td>
<td>❌</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Compatibilidad casi total con SQL Server</td>
<td>❌</td><td>❌</td><td>❌</td>
<td>❌</td><td>❌</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Compatible con Azure Hybrid Benefit</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

<tr>
<td>Compatible con Reserved Capacity</td>
<td>✅</td><td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
<td>✅</td><td>✅</td>
</tr>

</tbody>
</table>

Comparativa: PITR vs RPO vs RTO vs LTR

| Concepto | Significado              | ¿Es objetivo o tecnología? | ¿Qué mide / permite?                                                   | Ejemplo práctico                                           | En Azure SQL                                          |
| -------- | ------------------------ | -------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------- |
| **PITR** | Point-In-Time Restore    | Tecnología                 | Restaurar una base a un momento exacto dentro del período de retención | Restaurar la BD a las 10:04 antes de un borrado accidental | Incluido por defecto (7–35 días según configuración)  |
| **LTR**  | Long-Term Retention      | Tecnología                 | Retención de backups durante años                                      | Mantener backups 7 años por normativa                      | Configurable (hasta 10 años)                          |
| **RPO**  | Recovery Point Objective | Objetivo de negocio        | Cuánta pérdida de datos es aceptable                                   | “Máximo 5 minutos de pérdida”                              | Depende de arquitectura (BC ≈ 0, Geo-replication > 0) |
| **RTO**  | Recovery Time Objective  | Objetivo de negocio        | Cuánto tiempo puede tardar el sistema en volver a estar operativo      | “Debe estar disponible en 2 minutos”                       | HA síncrona → bajo RTO                                |

## Guía de Selección de Azure SQL  según Requisitos Técnicos -  

````yml
¿Necesita control total del sistema operativo
o configuración específica del motor SQL?
│
├── Sí → SQL Server on Azure Virtual Machines (IaaS)
│       │
│       ├── Tipo servicio: IaaS (VM + SQL instalado)
│       ├── Compatible 100% SQL Server: ✅ Sí (idéntico a on-prem)
│       │
│       ├── HA: ❌ No viene de serie
│       │       ├── Debe configurarse:
│       │       │       Always On Availability Groups
│       │       │       Failover Cluster Instance
│       │       │       Log Shipping
│       │       ├── RPO/RTO → Depende de tu configuración
│       │       └── Zero data loss → Solo si configuras síncrono
│       │
│       ├── DR: ❌ No viene de serie
│       │       ├── Se configura manualmente
│       │       ├── Azure Site Recovery
│       │       └── Always On entre regiones
│       │
│       ├── Backups: ❌ No automáticos (salvo extensión SQL IaaS Agent)
│       ├── Patching: ❌ Lo gestionas tú (o mantenimiento automático)
│       ├── Escalado: Manual
│       ├── Gestión: Completa responsabilidad tuya
│       │
│       └── Escenario ideal:
│               Lift-and-shift sin cambios
│               Requisitos muy específicos de SO
│               Versiones antiguas SQL
│               Control total de configuración
│
└── No →
        ¿Necesita compatibilidad casi total con SQL Server (SQL Agent, cross-DB, CLR)?
        │
        ├── Sí → Azure SQL Managed Instance
        │       │
        │       ├── Modelos disponibles:
        │       │       vCore → General Purpose / Business Critical
        │       │       DTU → ❌ No disponible
        │       │
        │       ├── HA: Sí (de serie)
        │       │       ├── Tipo redundancia:
        │       │       │       General Purpose → Locally redundant (asincrónica dentro región)
        │       │       │       Business Critical → Zone-redundant (síncrona)
        │       │       ├── RPO:
        │       │       │       General Purpose → > 0 segundos
        │       │       │       Business Critical → ≈ 0
        │       │       ├── RTO:
        │       │       │       General Purpose → Bajo (segundos)
        │       │       │       Business Critical → Muy bajo (segundos)
        │       │       ├── Réplicas:
        │       │       │       General Purpose → 1 réplica asincrónica interna 
        │       │       │       Business Critical → 3 réplicas síncronas (Always On AG)
        │       │       ├── Read-only endpoints:
        │       │       │       General Purpose → ❌ No
        │       │       │       Business Critical → ✅ Sí
        │       │       ├── Automatic failover with zero data loss:
        │       │       │       General Purpose → ❌ No (asincrónica)
        │       │       │       Business Critical → ✅ Sí (síncrona)
        │       │
        │       ├── DR: Sí (Auto-failover group / Geo-replication)
        │       │       ├── Tipo redundancia: Geo-replication (entre regiones)
        │       │       ├── Tipo sincronización → Asincrónica
        │       │       ├── RPO → Segundos a minutos (depende del lag)
        │       │       └── RTO → Minutos
        │       │       └── Requiere activación: Sí (no viene configurado)
        │       │       ├── Réplicas:
        │       │       │       Secundaria asincrónica en otra región
        │       │       ├── Read-only endpoint:
        │       │       │       ✅ Sí (si se configura secondary)
        │       │       └── Requiere activación: Sí
        │       │
        │       ├── Read replicas: Solo en Business Critical  (síncronas)
        │       ├── Backups: Automáticos + PITR + LTR
        │       ├── In-Memory OLTP: Solo en Business Critical
        │       │
        │       ├── Patching: Automático (gestionado por Azure)
        │       ├── Escalado sin downtime: Sí (con pequeño failover)
        │       ├── Escalado automático: No
        │       ├── Computación: Dedicada
        │       ├── Almacenamiento:
        │       │       General Purpose → remoto
        │       │       Business Critical → local SSD
        │       ├── Compatible 100% SQL Server: Muy alta compatibilidad
        │       ├── Reserved Capacity: Sí (vCore)
        │       ├── Azure Hybrid Benefit: Sí (vCore)
        │       │
        │       └── Escenario ideal:
        │               Migración lift-and-shift
        │               Aplicaciones legacy
        │               Necesita SQL Agent y cross-database
        │
        └── No →
              ¿Base de datos muy grande (varios TB hasta 100 TB+) o
              necesita escalar almacenamiento independientemente del cómputo?
              │
              ├── Sí → Azure SQL Database – Hyperscale
              │       │
              │       ├── Modelos disponibles:
              │       │       vCore → Hyperscale
              │       │       DTU → ❌ No disponible
              │       │
              │       ├── HA: Sí (arquitectura distribuida)
              │       │       └── Réplicas asincrónicas internas (log service + page servers)
              │       │       ├── Tipo redundancia: Zone-redundant interna
              │       │       ├── Sincronización: Asincrónica distribuida 
              │       │       └── Built-in: Sí
              │       │       └── Automatic failover with zero data loss: ❌ No (asincrónica)
              │       │       ├── Réplicas:
              │       │       │       Múltiples réplicas asincrónicas distribuidas
              │       │       ├── Read-only endpoints:
              │       │       │       ✅ Sí (múltiples)
              │       │       └── Automatic failover with zero data loss: ❌ No
              │       │       ├── RPO → > 0 
              │       │       └── RTO → Bajo (segundos)
              │       │
              │       ├── DR: Sí (Auto-failover group / Geo-replication)
              │       │       ├── Tipo redundancia: Geo-replication
              │       │       ├── Sincronización: Asincrónica
              │       │       └── Requiere activación: Sí
              │       │
              │       ├── Read replicas: Sí (múltiples,asincrónicas)
              │       ├── Backups: Automáticos (snapshots + PITR + LTR)
              │       ├── In-Memory OLTP: No
              │       │
              │       ├── Patching: Automático
              │       ├── Escalado sin downtime: Sí
              │       ├── Escalado automático: Solo almacenamiento
              │       ├── Computación: Dedicada
              │       ├── Almacenamiento: Arquitectura distribuida separada del cómputo
              │       ├── Compatible 100% SQL Server: No
              │       ├── Reserved Capacity: Sí (vCore)
              │       ├── Azure Hybrid Benefit: Sí (vCore)
              │       │
              │       └── Escenario ideal:
              │               Bases de datos muy grandes
              │               Crecimiento impredecible
              │               Workloads mixtos (OLTP + analítico)
              │               Necesita escalar almacenamiento rápidamente
              │
              └── No →
                    ¿Carga OLTP muy alta / In-Memory / baja latencia?
                    │
                    ├── Sí → Azure SQL Database – Business Critical
                    │       │
                    │       ├── Modelos disponibles:
                    │       │       vCore → Business Critical
                    │       │       DTU → Premium
                    │       │
                    │       ├── Equivalencia:
                    │       │       Premium (DTU) ⇄ Business Critical (vCore)
                    │       │
                    │       ├── HA: Sí (réplicas síncronas locales – Always On AG)
                    │       │       ├── Tipo redundancia: Zone-redundant
                    │       │       ├── Sincronización: Síncrona
                    │       │       ├── RPO → ≈ 0
                    │       │       └── RTO → Muy bajo (segundos)
                    │       │       └── Built-in: Sí
                    │       │       └── Automatic failover with zero data loss: ✅ Sí
                    │       │       ├── Réplicas:
                    │       │       │       3 réplicas síncronas (Always On AG)
                    │       │       ├── Read-only endpoints: ✅ Sí (hasta 3)
                    │       │       │       
                    │       │       └── Automatic failover with zero data loss:  ✅ Sí
                    │       │              
                    │       │ 
                    │       ├── DR: Sí (Auto-failover group / Geo-replication)
                    │       │       ├── Tipo redundancia: Geo-replication
                    │       │       ├── Sincronización: Asincrónica entre regiones
                    │       │       └── Requiere activación: Sí
                    │       │       ├── RPO → Segundos a minutos
                    │       │       └── RTO → Minutos
                    │       │
                    │       ├── Read replicas: Sí (hasta 3)
                    │       ├── Backups: Automáticos + PITR + LTR
                    │       ├── In-Memory OLTP: Sí
                    │       │
                    │       ├── Patching: Automático
                    │       ├── Escalado sin downtime: Sí (failover breve)
                    │       ├── Escalado automático: No
                    │       ├── Computación: Dedicada
                    │       ├── Almacenamiento: Local SSD
                    │       ├── Compatible 100% SQL Server: No (pero alta compatibilidad)
                    │       ├── Reserved Capacity: Sí (vCore)
                    │       ├── Azure Hybrid Benefit: Sí (vCore)
                    │       │
                    │       └── Escenario ideal:
                    │               Sistemas críticos
                    │               Alta concurrencia
                    │               Latencia mínima
                    │
                    └── No →
                          ¿Muchas bases con uso variable?
                          │
                          ├── Sí → Elastic Pool
                          │       │
                          │       ├── Modelos disponibles:
                          │       │       vCore → General Purpose / Business Critical
                          │       │       DTU → Basic / Standard / Premium
                          │       │
                          │       ├── Equivalencias:
                          │       │       Basic (DTU)    ≈ General Purpose bajo
                          │       │       Standard (DTU) ⇄ General Purpose (vCore)
                          │       │       Premium (DTU)  ⇄ Business Critical (vCore)
                          │       │
                          │       ├── HA: Sí (integrado)
                          │       │       ├── General Purpose → Locally redundant (asincrónica)
                          │       │       ├── Business Critical → Zone-redundant (síncrona)
                          │       │       ├── General Purpose → Asincrónica
                          │       │       │       RPO → > 0
                          │       │       │       RTO → Bajo
                          │       │       ├── Business Critical → Síncrona
                          │       │       │       RPO → ≈ 0
                          │       │       │       RTO → Muy bajo
                          │       │       └── Built-in: Sí
                          │       │       └── Automatic failover with zero data loss:
                          │       │       │       General Purpose → ❌ No
                          │       │       │       Business Critical / Premium → ✅ Sí
                          │       │       ├── Réplicas:
                          │       │       │       General Purpose → 1 asincrónica interna
                          │       │       │       Business Critical → 3 síncronas
                          │       │       ├── Read-only endpoints:
                          │       │       │       General Purpose → ❌ No
                          │       │       │       Business Critical → ✅ Sí
                          │       │       └── Automatic failover with zero data loss:
                          │       │               General Purpose → ❌ No
                          │       │               Business Critical → ✅ Sí
                          │       │     
                          │       ├── DR: Sí asincrónica entre regiones)
                          │       │       ├── Sincronización: Asincrónica
                          │       │       ├── RPO → Segundos a minutos
                          │       │       └── RTO → Minutos
                          │       ├── Read replicas: Solo si BC / Premium
                          │       ├── Backups: Automáticos
                          │       ├── In-Memory OLTP: Solo si BC / Premium
                          │       │
                          │       ├── Patching: Automático
                          │       ├── Escalado sin downtime: Sí
                          │       ├── Escalado automático: No
                          │       ├── Computación: Compartida entre DBs
                          │       ├── Almacenamiento: Remoto (GP) / Local (BC)
                          │       ├── Compatible 100% SQL Server: No
                          │       ├── Reserved Capacity: Sí (vCore)
                          │       ├── Azure Hybrid Benefit: Sí (vCore)
                          │       │
                          │       └── Escenario ideal:
                          │               SaaS multi-tenant
                          │               Muchas bases pequeñas
                          │               Optimización de costes
                          │
                          └── No →
                                ¿Carga intermitente / dev-test?
                                │
                                ├── Sí → Single DB – General Purpose Serverless
                                │       │
                                │       ├── Modelos disponibles:
                                │       │       vCore → General Purpose (Serverless)
                                │       │       DTU → ❌ No disponible
                                │       │
                                │       ├── Equivalente aproximado en DTU:
                                │       │       Standard
                                │       │
                                │       ├── HA: Sí (réplica asincrónica)
                                │       │       ├── RPO → > 0
                                │       │       └── RTO → Bajo
                                │       │       ├── Tipo redundancia: Locally redundant
                                │       │       └── Built-in: Sí
                                │       │       └── Automatic failover with zero data loss: ❌ No
                                │       │       ├── Réplicas:
                                │       │       │       1 asincrónica interna
                                │       │       ├── Read-only endpoints: ❌ No
                                │       │       └── Automatic failover with zero data loss: ❌ No
                                │       │               
                                │       │     
                                │       ├── DR: Sí (asincrónica)
                                │       │       ├── Tipo redundancia: Geo-replication
                                │       │       ├── Sincronización: Asincrónica  
                                │       │       ├── RPO → Segundos a minutos
                                │       │       └── RTO → Minutos
                                │       │       └── Requiere activación: Sí
                                │       │  
                                │       ├── Read replicas: No dedicadas
                                │       ├── Backups: Automáticos
                                │       ├── In-Memory OLTP: No
                                │       │
                                │       ├── Patching: Automático
                                │       ├── Escalado sin downtime: Sí
                                │       ├── Escalado automático: Sí (computación)
                                │       ├── Computación: Dedicada pero elástica
                                │       ├── Almacenamiento: Remoto
                                │       ├── Compatible 100% SQL Server: No
                                │       ├── Reserved Capacity: No
                                │       ├── Azure Hybrid Benefit: Sí
                                │       │
                                │       └── Escenario ideal:
                                │               Dev/Test
                                │               Workloads impredecibles
                                │               Uso intermitente
                                │               Optimización de costes
                                │
                                └── No → Single DB – General Purpose (Provisioned)
                                        │
                                        ├── Modelos disponibles:
                                        │       vCore → General Purpose
                                        │       DTU → Standard
                                        │
                                        ├── Equivalencia:
                                        │       Standard (DTU) ⇄ General Purpose (vCore)
                                        │
                                        ├── HA: Sí (réplica asincrónica)
                                        │       ├── Tipo redundancia: Locally redundant
                                        │       └── Built-in: Sí
                                        │       └── Automatic failover with zero data loss: ❌ No
                                        │       ├── Réplicas:
                                        │       │       1 asincrónica interna
                                        │       ├── Read-only endpoints: ❌ No
                                        │       └── Automatic failover with zero data loss: ❌ No
                                        │       ├── RPO → > 0
                                        │       └── RTO → Bajo
                                        │  
                                        ├── DR: Sí (asincrónica)
                                        │       ├── Tipo redundancia: Geo-replication
                                        │       ├── Sincronización: Asincrónica
                                        │       └── Requiere activación: Sí
                                        │       ├── RPO → Segundos a minutos
                                        │       └── RTO → Minutos
                                        │  
                                        ├── Read replicas: No dedicadas
                                        ├── Backups: Automáticos
                                        ├── In-Memory OLTP: No
                                        │
                                        ├── Patching: Automático
                                        ├── Escalado sin downtime: Sí
                                        ├── Escalado automático: No
                                        ├── Computación: Dedicada
                                        ├── Almacenamiento: Remoto
                                        ├── Compatible 100% SQL Server: No
                                        ├── Reserved Capacity: Sí (vCore)
                                        ├── Azure Hybrid Benefit: Sí (vCore)
                                        │
                                        └── Escenario ideal:
                                                Aplicaciones estándar
                                                OLTP moderado
            

````
## DR
-👉 Zone-redundant ≠ DR
-👉 DR es entre regiones (asincrónico)

- “Protect against zone-level failure” → Zone-redundant
- “Protect against datacenter hardware failure” → Locally redundant
- “Protect against regional outage” → Geo-replication / Failover group

| Tipo              | Protege contra           |
| ----------------- | ------------------------ |
| Locally redundant | Fallo de hardware        |
| Zone-redundant    | Fallo de zona completa   |
| Geo-replication   | Fallo de región completa |


## 🌳 Árbol de Herramientas de Migración a Azure SQL
````
Migración a Azure SQL
│
├── 1️⃣ Herramientas de evaluación / compatibilidad
│       │
│       └── SQL Server Migration Assistant (SSMA)
│               │
│               ├── Qué es:
│               │       Herramienta para convertir y migrar bases
│               │       desde otros motores a SQL Server/Azure SQL
│               │
│               ├── Orígenes soportados:
│               │       Oracle
│               │       MySQL
│               │       PostgreSQL
│               │       DB2
│               │       Access
│               │
│               ├── Función principal:
│               │       Analizar compatibilidad
│               │       Convertir esquema
│               │       Migrar datos
│               │
│               └── Uso típico:
│                       Migración heterogénea (no SQL Server)
│
│
├── 2️⃣ Herramientas de evaluación para SQL Server
│       │
│       └── Azure SQL Migration Extension (en Azure Data Studio)
│               │
│               ├── Qué es:
│               │       Extensión que analiza SQL Server
│               │       antes de migrar a Azure
│               │
│               ├── Función principal:
│               │       Evaluación de compatibilidad
│               │       Identificar problemas
│               │       Recomendar destino:
│               │             Azure SQL Database
│               │             Managed Instance
│               │             SQL en VM
│               │
│               └── Uso típico:
│                       Assessment previo a migración
│
│
├── 3️⃣ Orquestador de migración online/offline
│       │
│       └── Azure Database Migration Service (DMS)
│               │
│               ├── Qué es:
│               │       Servicio PaaS en Azure
│               │       que ejecuta la migración
│               │
│               ├── Soporta:
│               │       Migraciones online (mínimo downtime)
│               │       Migraciones offline
│               │
│               ├── Orígenes:
│               │       SQL Server
│               │       Oracle
│               │       MySQL
│               │       PostgreSQL
│               │
│               └── Uso típico:
│                       Migraciones productivas
│                       Migraciones con mínimo downtime
│
│
└── 4️⃣ Entorno cliente
        │
        └── Azure Data Studio
                │
                ├── Qué es:
                │       Cliente ligero para gestionar SQL
                │
                ├── No migra por sí solo
                │
                ├── Puede usar:
                │       Azure SQL Migration Extension
                │
                └── Uso típico:
                        Gestión y análisis

````
