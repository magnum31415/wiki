# Azure SQL Database 

Servicio PaaS basado en SQLServer totalmente gestionado 

## Mapa Jerárquico de Azure SQL (Servicios y Modelos de Compra)
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




## Guía de Selección de Azure SQL según Requisitos Técnicos Ampliado

````yml

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
│       │
│       ├── DR: Sí (Auto-failover group / Geo-replication)
│       │       ├── Tipo redundancia: Geo-replication (entre regiones)
│       │       ├── Tipo sincronización → Asincrónica
│       │       └── Requiere activación: Sí (no viene configurado)
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
│       │       GP → remoto
│       │       BC → local SSD
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
            │       │       └── Built-in: Sí
            │       │ 
            │       ├── DR: Sí (Auto-failover group / Geo-replication)
            │       │       ├── Tipo redundancia: Geo-replication
            │       │       ├── Sincronización: Asincrónica entre regiones
            │       │       └── Requiere activación: Sí
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
                  │       │       Basic (DTU)    ≈ GP bajo
                  │       │       Standard (DTU) ⇄ General Purpose (vCore)
                  │       │       Premium (DTU)  ⇄ Business Critical (vCore)
                  │       │
                  │       ├── HA: Sí (integrado)
                  │       │       ├── GP → Locally redundant (asincrónica)
                  │       │       ├── BC → Zone-redundant (síncrona)
                  │       │       └── Built-in: Sí
                  │       │     
                  │       ├── DR: Sí asincrónica entre regiones)
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
                        │       │       ├── Tipo redundancia: Locally redundant
                        │       │       └── Built-in: Sí
                        │       │     
                        │       ├── DR: Sí (asincrónica)
                        │       │       ├── Tipo redundancia: Geo-replication
                        │       │       ├── Sincronización: Asincrónica
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
                                │  
                                ├── DR: Sí (asincrónica)
                                │       ├── Tipo redundancia: Geo-replication
                                │       ├── Sincronización: Asincrónica
                                │       └── Requiere activación: Sí
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
                                        Coste equilibrado


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
