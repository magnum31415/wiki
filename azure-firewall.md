[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


````
Azure Firewall
    │
    ▼
Firewall Policy
    │
    ▼
Rule Collection Group
    │
    ├── Network Rule Collection
    │       └── Rules
    │
    ├── Application Rule Collection
    │       └── Rules
    │
    └── NAT Rule Collection
            └── Rules
````

````
Azure Firewall (Azure Firewall Resource)
│
└── Firewall Policy
    │
    └── fwp-hub-germanywestcentral (Firewall Policy)
        │
        ├── fwpcg-base-gwc (Rule Collection Group)
        │   └── Reglas comunes de plataforma
        │
        ├── fwpcg-docscan-prod-gwc (Rule Collection Group)
        │   │
        │   ├── rc-net-allow-docscan-to-sql (Network Rule Collection)
        │   │   ├── allow-docscan-sql-prod (Rule)
        │   │   └── allow-docscan-sql-reporting (Rule)
        │   │
        │   ├── rc-app-allow-docscan-internet (Application Rule Collection)
        │   │   ├── allow-api-vendor (Rule)
        │   │   └── allow-microsoft-login (Rule)
        │   │
        │   └── rc-nat-docscan-publish (NAT Rule Collection)
        │       └── publish-docscan-web (Rule)
        │
        └── fwpcg-app001-prod-gwc (Rule Collection Group)
            │
            ├── rc-net-allow-app001 (Network Rule Collection)
            │   └── allow-app001-to-db (Rule)
            │
            ├── rc-app-allow-app001 (Application Rule Collection)
            │   └── allow-app001-api (Rule)
            │
            └── rc-nat-app001 (NAT Rule Collection)
                └── publish-app001-web (Rule)
````
