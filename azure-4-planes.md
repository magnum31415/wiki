# Los cuatro planos (Planes) de Azure

Azure puede entenderse como cuatro grandes planos funcionales:

1. **Identity Plane**
2. **Control Plane (Management Plane)**
3. **Data Plane**
4. **Billing Plane (Commerce Plane)**

Cada uno responde a una pregunta diferente.

![Diagrama 4 Planes](./img/azure/azure-4-planes.png)


---

# Arquitectura general

```text
                   Usuario / Aplicación
                            │
                            ▼
                  Microsoft Entra ID
                     (Identity Plane)
                            │
                     Token OAuth/JWT
                            │
        ┌───────────────────┴───────────────────┐
        ▼                                       ▼

Control Plane                         Data Plane
(Azure Resource Manager)            (Servicios Azure)

        │                                   │

        ▼                                   ▼

Microsoft.Compute                  Blob Storage
Microsoft.Network                  Key Vault
Microsoft.Storage                  Azure SQL
Microsoft.Web                      Cosmos DB
Microsoft.KeyVault                 Service Bus
...                                ...

        │                                   │

        ▼                                   ▼

Administrar recursos              Leer / escribir datos

        │

        ▼

Billing / Commerce Plane

Cost Management
Facturación
Budgets
Reservations
Invoices
```

---

# 1. Identity Plane

## ¿Qué es?

Es el plano encargado de la **identidad**.

Su componente principal es:

- **Microsoft Entra ID**

Su misión es responder:

> **¿Quién eres?**

---

## Arquitectura

```text
Clientes

Portal

Azure CLI

PowerShell

Aplicaciones

Usuarios

        │

        ▼

Microsoft Entra ID

        │

        ├── Autenticación

        ├── MFA

        ├── OAuth2

        ├── OpenID Connect

        ├── Conditional Access

        ├── PIM

        └── Identity Protection

        │

        ▼

Token JWT
```

---

## Ejemplos

- Login en Azure Portal
- Login mediante Azure CLI
- Autenticación OAuth2
- MFA
- Obtener un Access Token

---

# 2. Control Plane (Management Plane)

## ¿Qué es?

Es el plano encargado de **administrar los recursos de Azure**.

Su componente principal es:

- **Azure Resource Manager (ARM)**

También recibe estos nombres:

- Control Plane
- Management Plane
- Azure Resource Manager (ARM)

En el contexto del AZ-104 pueden considerarse equivalentes.

Su misión es responder:

> **¿Qué recursos puedo administrar?**

---

## Arquitectura

```text
Clientes

Azure Portal

Azure CLI

Azure PowerShell

Terraform

Bicep

ARM Templates

REST API

SDK

Azure DevOps

GitHub Actions

        │

        ▼

Azure Resource Manager (ARM)

        │

        ▼

Microsoft.Compute

Microsoft.Network

Microsoft.Storage

Microsoft.KeyVault

Microsoft.Web

Microsoft.Sql

...

        │

        ▼

Recursos Azure
```

---

## Ejemplos

- Crear VM
- Crear Storage Account
- Crear VNet
- Crear NSG
- Asignar RBAC
- Eliminar un Resource Group

---

# 3. Data Plane

## ¿Qué es?

Es el plano encargado de acceder al **contenido de los recursos**.

No utiliza Azure Resource Manager.

Cada servicio expone su propia API.

Su misión es responder:

> **¿Qué datos puedo utilizar?**

---

## Arquitectura

```text
Clientes

Aplicaciones

SDK

REST API

Azure Storage Explorer

AzCopy

Azure CLI

PowerShell

        │

        ▼

Servicio Azure

Blob Storage

Key Vault

Azure SQL

Cosmos DB

Service Bus

...

        │

        ▼

Datos
```

---

## Ejemplos

- Leer un Blob
- Escribir un Blob
- Obtener un secreto de Key Vault
- Ejecutar una consulta SQL
- Publicar un mensaje en Service Bus

---

# 4. Billing Plane (Commerce Plane)

## ¿Qué es?

Es el plano encargado de la **facturación y costes**.

No existe un equivalente a ARM.

Los servicios principales son:

- Azure Billing
- Azure Cost Management
- Consumption API
- Commerce API

Su misión es responder:

> **¿Cuánto cuesta?**

---

## Arquitectura

```text
Clientes

Portal

REST API

Power BI

Azure CLI

        │

        ▼

Billing

Cost Management

Consumption

Commerce

        │

        ▼

Facturas

Budgets

Reservations

Cost Analysis
```

---

## Ejemplos

- Consultar la factura mensual
- Crear un presupuesto (Budget)
- Analizar costes
- Consultar consumo
- Administrar Reservations

---

# Comparativa

| Plano | Backend principal | Pregunta que responde |
|--------|-------------------|-----------------------|
| **Identity Plane** | Microsoft Entra ID | ¿Quién eres? |
| **Control Plane (Management Plane)** | Azure Resource Manager (ARM) | ¿Qué recursos puedes administrar? |
| **Data Plane** | API específica de cada servicio Azure | ¿Qué datos puedes utilizar? |
| **Billing Plane (Commerce Plane)** | Azure Billing / Cost Management | ¿Cuánto cuesta? |

---

# Comparación de operaciones

| Operación | Plano |
|-----------|--------|
| Iniciar sesión en Azure | Identity Plane |
| Obtener un token OAuth | Identity Plane |
| Crear una VM | Control Plane |
| Eliminar una Storage Account | Control Plane |
| Crear una VNet | Control Plane |
| Asignar un rol RBAC | Control Plane |
| Leer un Blob | Data Plane |
| Escribir un Blob | Data Plane |
| Leer un secreto de Key Vault | Data Plane |
| Ejecutar una consulta SQL | Data Plane |
| Consultar la factura | Billing Plane |
| Crear un Budget | Billing Plane |
| Analizar costes | Billing Plane |

---

# Regla mnemotécnica

| Plano | Frase para recordar |
|--------|---------------------|
| **Identity Plane** | **¿Quién eres?** |
| **Control Plane** | **¿Qué recursos puedes administrar?** |
| **Data Plane** | **¿Qué datos puedes utilizar?** |
| **Billing Plane** | **¿Cuánto cuesta?** |

---

# Resumen

```text
Identity Plane
        │
        ▼
¿Quién eres?

        │
        ▼
Control Plane
        │
        ▼
¿Qué recursos administras?

        │
        ▼
Data Plane
        │
        ▼
¿Qué datos utilizas?

        │
        ▼
Billing Plane
        │
        ▼
¿Cuánto cuesta?
```
