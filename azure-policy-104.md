[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

## Index

- [Minimum Azure Policy JSON](#minimum-azure-policy-json)
- [Required fields](#required-fields)
- [Common optional fields](#common-optional-fields)
- [Typical (but not minimal) policy example](#typical-but-not-minimal-policy-example)
- [Summary](#summary)
- [Azure Service Endpoint Policy (AZ-104)](#azure-service-endpoint-policy-az-104)
---

## Minimum Azure Policy JSON

The **minimum valid JSON document for an Azure Policy** requires only `properties`, `displayName`, and `policyRule` with an `if` condition and a `then` effect.

```json
{
  "properties": {
    "displayName": "Minimal policy",
    "policyRule": {
      "if": {
        "field": "type",
        "equals": "Microsoft.Resources/subscriptions/resourceGroups"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## Required fields

- **properties**
- **displayName**
- **policyRule**
- **if**
- **then.effect**

## Common optional fields

These fields are commonly included but are **not required**:

```json
"description": "Policy description",
"mode": "All",
"parameters": {}
```

## Typical (but not minimal) policy example

```json
{
  "properties": {
    "displayName": "Deny specific resource type",
    "description": "Example policy",
    "mode": "All",
    "policyRule": {
      "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## Summary

The **absolute minimal structure** for an Azure Policy is:

- `properties`
- `displayName`
- `policyRule`
- `if`
- `then.effect`

---
# Azure Service Endpoint Policy (AZ-104)

---

# Qué es Service Endpoint Policy

`Service Endpoint Policy` es una feature avanzada de red Azure relacionada con:

```text
Virtual Network Service Endpoints
```

y sirve para:

```text
restringir a qué recursos PaaS concretos
puede acceder una subnet
```

---

# Qué hace un Service Endpoint normal

Un:

```text
Service Endpoint
```

permite que una subnet acceda de forma privada/optimizada a servicios PaaS Azure:

- Storage Accounts
- SQL
- CosmosDB
- etc.

SIN usar Internet pública.

---

# Problema del Service Endpoint normal

Si habilitas:

```text
Microsoft.Storage
```

en una subnet:

↓

esa subnet puede acceder a:

```text
CUALQUIER Storage Account Azure
```

(si firewall/ACL lo permiten).

---

# Qué añade Service Endpoint Policy

Permite restringir:

```text
qué Storage Accounts específicas
pueden usarse
```

desde esa subnet.

---

# Ejemplo práctico

## Sin Policy

Subnet:

```text
subnet-app
```

↓

Puede acceder a:

- storage1
- storage2
- storage-random-external

---

## Con Service Endpoint Policy

Policy:

```text
solo storage-finance-prod
```

↓

Ahora la subnet SOLO puede acceder a:

```text
storage-finance-prod
```

---

# Concepto clave

Service Endpoint Policy añade:

```text
control granular
```

sobre:

```text
Service Endpoints
```

---

# Servicios soportados

| Servicio | Compatible |
|---|---|
| Azure Storage | ✅ |
| Otros servicios | Muy limitado/no común |

---

# Arquitectura conceptual

```text
Subnet
   ↓
Service Endpoint
   ↓
Service Endpoint Policy
   ↓
Allowed Storage Accounts
```

---

# Por qué tiene región

Porque:

```text
Service Endpoint Policies son recursos regionales
```

igual que:
- VNets
- NSGs
- Route Tables

---

# Relación con VNets

La policy se asocia a:

```text
subnets concretas
```

y las subnets pertenecen a:

```text
VNets regionales
```

---

# Importante examen

La policy debe existir en:

```text
la misma región que la VNet/subnet
```

---

# Ejemplo

## VNet

```text
West Europe
```

↓

La:

```text
Service Endpoint Policy
```

también debe estar en:

```text
West Europe
```

---

# Muy importante examen

La región de la policy:

```text
NO depende del Storage Account
```

depende de:

```text
la VNet/subnet
```

---

# Trampa típica AZ-104

Pensar que:

```text
la policy debe estar en la región del Storage Account
```

❌ Incorrecto.

---

# Diferencia importante

| Feature | Función |
|---|---|
| Service Endpoint | Conectar subnet a servicio Azure |
| Service Endpoint Policy | Restringir recursos permitidos |

---

# Comparación con Private Endpoint

| Feature | Nivel |
|---|---|
| Service Endpoint | Servicio PaaS completo |
| Service Endpoint Policy | Filtrado granular |
| Private Endpoint | NIC privada directa |

---

# Caso típico uso

## Empresa

Subnet producción:

```text
subnet-prod
```

↓

Solo debe acceder a:

```text
storage-prod-company
```

NO:
- storage externos
- otras subscripciones
- storage personales

↓

Se usa:

```text
Service Endpoint Policy
```

---

# Regla rápida examen

```text
Service Endpoint Policies restrict which Azure Storage accounts can be accessed through a Service Endpoint.
```

---

# Frases clave AZ-104

```text
Service Endpoint Policies provide granular access control for Service Endpoints.
```

```text
Service Endpoint Policies are regional resources.
```

```text
The policy region must match the VNet/subnet region.
```

