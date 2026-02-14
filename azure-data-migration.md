[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Migración de datos On-Prem → Azure
````
¿Tienes buena conectividad de red y suficiente ancho de banda?
│
├── Sí (migración online por red)
│       │
│       ├── ¿Necesitas sincronización continua?
│       │
│       │       ├── Sí →
│       │       │       Azure File Sync
│       │       │       Azure Storage Sync
│       │       │       Azure Data Box Gateway
│       │       │       Azure Database Migration Service (para BBDD)
│       │       │
│       │       └── No →
│       │               AzCopy
│       │               Azure Storage Explorer
│       │               Azure Migrate (VMs)
│       │               Database Migration Service (one-time cutover)
│       │
│       └── ¿Es Big Data / Data Lake?
│               └── Azure Data Factory
│
└── No (ancho de banda limitado o datos masivos)
        │
        ├── ¿Transferencia puntual offline?
        │
        │       ├── Hasta ~80 TB →
        │       │       Azure Import/Export Service
        │       │
        │       └── De 40 TB a PB →
        │               Azure Data Box (Dispositivo físico Microsoft)
        │
        └── ¿Necesitas procesamiento en Edge?
                └── Azure Data Box Edge

````
