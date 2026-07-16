[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [→ Azure Firewall y Virtual Networks (AZ-104)](#azure-firewall-y-virtual-networks-az-104)

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
---

# Azure Firewall y Virtual Networks (AZ-104)

Un **Azure Firewall** solo puede conectarse a una **Virtual Network (VNet)** que cumpla los siguientes requisitos:

- Debe estar en la **misma suscripción**.
- Debe estar en la **misma región**.
- Debe estar en el **mismo Resource Group** que el Azure Firewall.

La única excepción es la **Public IP**, que **puede estar en un Resource Group diferente**.

---

# Requisitos

| Recurso | Misma suscripción | Misma región | Mismo Resource Group |
|----------|:-----------------:|:------------:|:--------------------:|
| Azure Firewall ↔ VNet | ✅ | ✅ | ✅ |
| Azure Firewall ↔ Public IP | ✅ | ❌ No necesario | ❌ No necesario |

---

# Ejemplo

## Correcto

```text
Subscription1

RG1
├── Azure Firewall
├── VNet1
└── AzureFirewallSubnet

RG2
└── Public IP
```

✅ Configuración válida.

---

## Incorrecto

```text
Subscription1

RG1
└── Azure Firewall

RG2
└── VNet1
```

❌ No es válido porque el **Firewall y la VNet están en Resource Groups distintos**.

---

> [!IMPORTANT]
> **Clave para el AZ-104**
>
> El **Azure Firewall** y la **VNet** deben estar:
>
> - En la **misma suscripción**.
> - En la **misma región**.
> - En el **mismo Resource Group**.
>
> La **Public IP** del Firewall **sí puede estar en otro Resource Group**.
