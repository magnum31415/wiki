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

## Tabla Comparativa – Capacidades Azure SQL

| Característica                                | Azure SQL Database Single Database – General Purpose | Azure SQL Database Single Database – Business Critical | Azure SQL Database Single Database – Hyperscale | Azure SQL Database Elastic Pool – General Purpose | Azure SQL Database Elastic Pool – Business Critical | Azure SQL Managed Instance – General Purpose | Azure SQL Managed Instance – Business Critical |
| --------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------- | --------------------------------------------------- | -------------------------------------------- | ---------------------------------------------- |
| Modelo basado en vCore                        | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Modelo basado en DTU disponible               | ✅                                                    | ❌                                                      | ❌                                               | ✅                                                 | ❌                                                   | ❌                                            | ❌                                              |
| In-Memory OLTP                                | ❌                                                    | ✅                                                      | ❌                                               | ❌                                                 | ✅                                                   | ❌                                            | ✅                                              |
| Memory-Optimized Tables                       | ❌                                                    | ✅                                                      | ❌                                               | ❌                                                 | ✅                                                   | ❌                                            | ✅                                              |
| Columnstore Indexes                           | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Alta disponibilidad integrada                 | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Recuperación ante desastres (Geo-replication) | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Auto-failover groups                          | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Réplicas de solo lectura dedicadas            | ❌                                                    | ✅                                                      | ✅                                               | ❌                                                 | ✅                                                   | ❌                                            | ✅                                              |
| Escalado sin downtime                         | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Escalado automático de computación            | ✅ (solo modo Serverless)                             | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ❌                                            | ❌                                              |
| Modo Serverless disponible                    | ✅                                                    | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ❌                                            | ❌                                              |
| Auto-pause disponible                         | ✅                                                    | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ❌                                            | ❌                                              |
| Almacenamiento local SSD                      | ❌                                                    | ✅                                                      | ❌ (arquitectura distribuida)                    | ❌                                                 | ✅                                                   | ❌                                            | ✅                                              |
| Almacenamiento remoto                         | ✅                                                    | ❌                                                      | ✅                                               | ✅                                                 | ❌                                                   | ✅                                            | ❌                                              |
| Recursos dedicados                            | ✅                                                    | ✅                                                      | ✅                                               | ❌ (compartidos)                                   | ❌ (compartidos)                                     | ✅                                            | ✅                                              |
| SQL Agent disponible                          | ❌                                                    | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ✅                                            | ✅                                              |
| Consultas cross-database completas            | ❌                                                    | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ✅                                            | ✅                                              |
| Compatibilidad casi total con SQL Server      | ❌                                                    | ❌                                                      | ❌                                               | ❌                                                 | ❌                                                   | ✅                                            | ✅                                              |
| Compatible con Azure Hybrid Benefit           | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |
| Compatible con Reserved Capacity              | ✅                                                    | ✅                                                      | ✅                                               | ✅                                                 | ✅                                                   | ✅                                            | ✅                                              |



## Guía de Selección de Azure SQL según Requisitos Técnicos Ampliado

````yml
¿Necesita compatibilidad casi total con SQL Server (SQL Agent, cross-DB, CLR)?
│
├── Sí → Azure SQL Managed Instance
│       │
│       ├── HA: Sí (Always On interno)
│       ├── DR: Sí (Auto-failover group / Geo-replication)
│       ├── Read replicas: Solo en Business Critical
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
│       ├── Reserved Capacity: Sí
│       ├── Azure Hybrid Benefit: Sí
│       │
│       └── Escenario ideal:
│               Migración lift-and-shift
│               Aplicaciones legacy
│               Necesita SQL Agent y cross-database
│
└── No →
      ¿Carga OLTP muy alta / In-Memory / baja latencia?
      │
      ├── Sí → Azure SQL Database – Business Critical
      │       │
      │       ├── HA: Sí (réplicas síncronas locales)
      │       ├── DR: Sí (Auto-failover group / Geo-replication)
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
      │       ├── Reserved Capacity: Sí
      │       ├── Azure Hybrid Benefit: Sí
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
            │       ├── HA: Sí (integrado)
            │       ├── DR: Sí
            │       ├── Read replicas: Solo si BC
            │       ├── Backups: Automáticos
            │       ├── In-Memory OLTP: Solo si BC
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
                  │       ├── HA: Sí
                  │       ├── DR: Sí
                  │       ├── Read replicas: No dedicadas
                  │       ├── Backups: Automáticos
                  │       ├── In-Memory OLTP: No
                  │       │
                  │       ├── Patching: Automático
                  │       ├── Escalado sin downtime: Sí
                  │       ├── Escalado automático: Sí
                  │       ├── Auto-pause: Sí
                  │       ├── Computación: Dedicada pero dinámica
                  │       ├── Almacenamiento: Remoto
                  │       ├── Compatible 100% SQL Server: No
                  │       ├── Reserved Capacity: No
                  │       ├── Azure Hybrid Benefit: Sí
                  │       │
                  │       └── Escenario ideal:
                  │               Dev/Test
                  │               Workloads impredecibles
                  │               Optimización de costes
                  │
                  └── No → Single DB – General Purpose (Provisioned)
                          │
                          ├── HA: Sí
                          ├── DR: Sí
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
                          ├── Reserved Capacity: Sí
                          ├── Azure Hybrid Benefit: Sí
                          │
                          └── Escenario ideal:
                                  Aplicaciones estándar
                                  OLTP moderado
                                  Coste equilibrado

````
