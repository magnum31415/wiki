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
│       ├── HA: Sí (Always On interno)
│       ├── DR: Sí (Auto-failover groups / Geo-replication)
│       ├── Read replicas: Sí (Business Critical)
│       ├── Backups: Automáticos + PITR + LTR
│       └── In-Memory OLTP: Sí (Business Critical)
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
      │       └── In-Memory OLTP: Sí
      │
      └── No →
            ¿Muchas bases con uso variable?
            │
            ├── Sí → Elastic Pool
            │       │
            │       ├── HA: Sí (integrado)
            │       ├── DR: Sí (Geo-replication)
            │       ├── Read replicas: Solo si Business Critical
            │       ├── Backups: Automáticos
            │       └── In-Memory OLTP: Solo en BC
            │
            └── No →
                  ¿Carga intermitente / dev-test?
                  │
                  ├── Sí → Single DB – General Purpose Serverless
                  │       │
                  │       ├── HA: Sí (remota)
                  │       ├── DR: Sí
                  │       ├── Read replicas: No dedicadas
                  │       ├── Auto-pause: Sí
                  │       └── In-Memory OLTP: No
                  │
                  └── No → Single DB – General Purpose
                          │
                          ├── HA: Sí (réplica remota)
                          ├── DR: Sí
                          ├── Read replicas: No dedicadas
                          ├── Backups: Automáticos
                          └── In-Memory OLTP: No

````

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
