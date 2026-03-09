[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# EntraID 

## Index

- [Microsoft Active Directory (On-Premises) vs Microsoft Entra ID](#microsoft-active-directory-on-premises-vs-microsoft-entra-id)
  - [Overview](#overview)
  - [Architecture](#architecture)
  - [Identity Management](#identity-management)
  - [Device Management](#device-management)
  - [Authentication and Security](#authentication-and-security)
  - [Integration](#integration)
  - [Typical Use Cases](#typical-use-cases)
  - [Hybrid Identity (Common Scenario)](#hybrid-identity-common-scenario)
  - [Summary](#summary)

- [Active Directory vs Microsoft Entra ID](#active-directory-vs-microsoft-entra-id)
  - [¿Son lo mismo?](#son-lo-mismo)
  - [Diferencia fundamental](#diferencia-fundamental)
  - [No son intercambiables](#no-son-intercambiables)
  - [Ejemplo práctico](#ejemplo-práctico)
  - [Modelo más común: Identidad híbrida](#modelo-más-común-identidad-híbrida)

- [Microsoft Entra ID Plans and Pricing](#microsoft-entra-id-plans-and-pricing)
  - [Feature Comparison](#feature-comparison)
  - [Typical Use Cases](#typical-use-cases)

- [Azure Account Types and Tenant Creation](#azure-account-types-and-tenant-creation)
  - [1. Azure Free Account](#1-azure-free-account)
  - [2. Pay-As-You-Go Account](#2-pay-as-you-go-account)
  - [Resumen](#resumen)

- [Crear una Infraestructura Azure desde Cero (Empresa)](#crear-una-infraestructura-azure-desde-cero-empresa)
  - [Orden recomendado](#orden-recomendado)
  - [1. Crear el Microsoft Entra ID Tenant](#1-crear-el-microsoft-entra-id-tenant)
  - [2. Añadir el dominio corporativo](#2-añadir-el-dominio-corporativo)
  - [3. Crear las suscripciones](#3-crear-las-suscripciones)
  - [4. Crear la jerarquía de Management Groups](#4-crear-la-jerarquía-de-management-groups)
  - [5. Configurar RBAC](#5-configurar-rbac)
  - [6. Aplicar Azure Policies](#6-aplicar-azure-policies)
  - [7. Diseñar la red](#7-diseñar-la-red)
  - [8. Crear Resource Groups](#8-crear-resource-groups)
  - [9. Desplegar recursos](#9-desplegar-recursos)

- [¿Para crear el tenant tengo que crear primero la suscripción?](#para-crear-el-tenant-tengo-que-crear-primero-la-suscripción)

- [Orden Real para Crear un Entorno Azure desde Cero - Basado en Microsoft Cloud Adoption Framework](#orden-real-para-crear-un-entorno-azure-desde-cero---basado-en-microsoft-cloud-adoption-framework)
  - [1. Crear el Microsoft Entra ID Tenant](#1-crear-el-microsoft-entra-id-tenant-1)
  - [2. Asegurar la identidad](#2-asegurar-la-identidad)
  - [3. Crear las primeras Suscripciones](#3-crear-las-primeras-suscripciones)
  - [4. Crear la jerarquía de Management Groups](#4-crear-la-jerarquía-de-management-groups-1)
  - [5. Crear las suscripciones de plataforma](#5-crear-las-suscripciones-de-plataforma)
  - [6. Diseñar la red base](#6-diseñar-la-red-base)
  - [7. Aplicar Gobernanza](#7-aplicar-gobernanza)
  - [8. Configurar Observabilidad](#8-configurar-observabilidad)
  - [9. Crear las Landing Zones de Aplicación](#9-crear-las-landing-zones-de-aplicación)
  - [10. Desplegar Workloads](#10-desplegar-workloads)
  - [Jerarquía final completa de Azure](#jerarquía-final-completa-de-azure)
  - [Regla importante](#regla-importante)
  - [Error típico cuando se empieza con Azure](#error-típico-cuando-se-empieza-con-azure)

- [El problema actual](#1️⃣-el-problema-actual)
- [Buen patrón recomendado por Microsoft](#2️⃣-buen-patrón-recomendado-por-microsoft)
- [Cómo arreglar las subscriptions actuales](#3️⃣-cómo-arreglar-las-subscriptions-actuales)
  - [Paso 1 — crear grupo de administradores](#paso-1--crear-grupo-de-administradores)
  - [Paso 2 — asignar el grupo como Owner](#paso-2--asignar-el-grupo-como-owner)
  - [Paso 3 — eliminar al usuario antiguo](#paso-3--eliminar-al-usuario-antiguo)
- [Cómo deberían crearse las subscriptions nuevas](#4️⃣-cómo-deberían-crearse-las-subscriptions-nuevas)
  - [Modelo 1 — Enterprise Agreement / MCA](#modelo-1--enterprise-agreement--mca-el-más-común)
  - [Modelo 2 — Platform subscription creation](#modelo-2--platform-subscription-creation)
  - [Modelo 3 — Azure Landing Zone pattern](#modelo-3--azure-landing-zone-pattern)
- [Buenas prácticas clave](#5️⃣-buenas-prácticas-clave)
- [Algo importante que deberías revisar ahora](#6️⃣-algo-importante-que-deberías-revisar-ahora)
- [Arquitectura final recomendada](#7️⃣-arquitectura-final-recomendada)
---

# Microsoft Active Directory (On-Premises) vs Microsoft Entra ID
[Back to Index](#index)


## Overview
[Back to Index](#index)

| Feature | Microsoft Active Directory (On-Premises) | Microsoft Entra ID |
|---|---|---|
| Type | On-premises directory service | Cloud identity and access management service |
| Former Name | Active Directory Domain Services (AD DS) | Azure Active Directory (Azure AD) |
| Deployment | Installed on Windows Server in local infrastructure | Hosted and managed by Microsoft in Azure |
| Primary Purpose | Manage identities and devices inside a corporate network | Manage identities for cloud applications and services |

## Architecture
[Back to Index](#index)

| Aspect | Active Directory (AD DS) | Microsoft Entra ID |
|---|---|---|
| Infrastructure | Requires domain controllers | No servers required |
| Network dependency | Designed for internal networks (LAN) | Designed for internet and cloud access |
| Protocols | LDAP, Kerberos, NTLM | OAuth2, OpenID Connect, SAML |
| Authentication model | Domain-based authentication | Token-based authentication |

## Identity Management
[Back to Index](#index)

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Users and Groups | Yes | Yes |
| Organizational Units (OU) | Yes | No |
| Group Policy (GPO) | Yes | No |
| Role-Based Access Control | Limited | Yes (RBAC built-in) |
| Conditional Access | No | Yes |


## Device Management
[Back to Index](#index)

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Domain Join | Yes | No |
| Entra ID Join | No | Yes |
| Hybrid Join | Yes (with Entra integration) | Yes |
| Device Management | Group Policy | Intune / MDM |


## Authentication and Security
[Back to Index](#index)

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Kerberos Authentication | Yes | No |
| Multi-Factor Authentication | Limited | Yes |
| Conditional Access | No | Yes |
| Identity Protection | No | Yes |
| Single Sign-On | Internal SSO | Cloud SSO |

## Integration
[Back to Index](#index)

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| On-premises applications | Yes | Limited |
| Cloud applications | Limited | Yes |
| Microsoft 365 integration | Requires synchronization | Native |
| Hybrid identity | With Entra Connect | Native support |

## Typical Use Cases
[Back to Index](#index)

| Active Directory | Microsoft Entra ID |
|---|---|
| Managing internal corporate networks | Identity management for cloud services |
| Windows domain authentication | Microsoft 365 authentication |
| Managing servers and workstations | SaaS application access |
| Group Policy configuration | Conditional access and identity security |

## Hybrid Identity (Common Scenario)
[Back to Index](#index)

Many organizations use **both systems together**.

Architecture example:

````
On-premises Active Directory
│
│ (Azure AD Connect / Entra Connect)
│
Microsoft Entra ID
│
├── Microsoft 365
├── Azure resources
└── SaaS applications
````


In this model:

- **Active Directory** manages internal infrastructure.
- **Microsoft Entra ID** manages cloud authentication and access.


## Summary
[Back to Index](#index)

| Aspect | Active Directory | Microsoft Entra ID |
|---|---|---|
| Environment | On-premises | Cloud |
| Authentication | Kerberos / LDAP | OAuth / SAML / OpenID |
| Infrastructure | Requires domain controllers | Fully managed by Microsoft |
| Device management | Group Policy | Intune / MDM |
| Cloud integration | Limited | Native |

**Active Directory** is designed for traditional on-premises environments, while **Microsoft Entra ID** is built for cloud identity and modern authentication scenarios.

---

# Active Directory vs Microsoft Entra ID
[Back to Index](#index)

## ¿Son lo mismo?
[Back to Index](#index)

No. **No son lo mismo y no son intercambiables**, aunque ambos gestionen identidades.

Tienen **arquitecturas, protocolos y objetivos diferentes**.

## Diferencia fundamental
[Back to Index](#index)

| Aspecto | Active Directory (AD DS) | Microsoft Entra ID |
|---|---|---|
| Tipo | Directorio **on-premises** | Directorio **cloud** |
| Ubicación | Servidores Windows dentro de la red corporativa | Servicio gestionado por Microsoft en Azure |
| Arquitectura | **Jerárquica basada en dominios** (forest, domains, OUs, domain controllers) | **Arquitectura cloud multi-tenant** basada en **tenants e identidades** |
| Protocolos | **LDAP, Kerberos, NTLM, DNS** | **OAuth2, OpenID Connect, SAML, SCIM** |
| Autenticación | Autenticación **basada en dominio** (Kerberos tickets) | Autenticación **basada en tokens** |
| Objetivo | Gestionar **identidades, dispositivos y recursos dentro de la red corporativa** | Gestionar **identidades y acceso a aplicaciones cloud y SaaS** |
| Diseño | Pensado para **redes internas (LAN)** | Pensado para **internet y aplicaciones cloud** |
| Infraestructura | Requiere **Domain Controllers** | No requiere servidores |
| Qué tienen en común | Ambos son **servicios de identidad y directorio** que gestionan **usuarios, grupos y autenticación** | Ambos permiten **gestionar identidades, controlar accesos y aplicar políticas de seguridad** |


## No son intercambiables
[Back to Index](#index)

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Domain Join clásico | ✅ | ❌ |
| Group Policy (GPO) | ✅ | ❌ |
| LDAP | ✅ | ❌ |
| Kerberos | ✅ | ❌ |
| Gestión de servidores Windows | ✅ | ❌ |
| Conditional Access | ❌ | ✅ |
| MFA integrado | ❌ | ✅ |
| Identity Protection | ❌ | ✅ |
| Gestión de acceso a aplicaciones SaaS | ❌ | ✅ |
| Integración nativa con Microsoft 365 | ❌ | ✅ |

## Ejemplo práctico
[Back to Index](#index)

| Scenario | Identity System | Authentication | Infrastructure |
|---|---|---|---|
| **Login en un servidor Windows interno** | Active Directory | Kerberos | Domain Controller |
| **Login en Microsoft 365** | Microsoft Entra ID | OAuth / Tokens | Cloud authentication |


## Modelo más común: Identidad híbrida
[Back to Index](#index)


La mayoría de empresas utilizan **ambos sistemas conectados**.

---

# Microsoft Entra ID Plans and Pricing
[Back to Index](#index)

| Plan | Price (approx.) | Target Use | Key Features |
|---|---|---|---|
| **Free** | Free | Basic identity management for Azure and Microsoft services | User and group management, Single Sign-On (SSO) for Azure and some SaaS apps, Basic security reports, Self-service password change |
| **Microsoft Entra ID P1** | ~ $6 user/month | Enterprise identity management | Conditional Access, Dynamic groups, Self-service password reset (SSPR) for cloud and on-prem users, Hybrid identity with Entra Connect, Advanced group management |
| **Microsoft Entra ID P2** | ~ $9 user/month | Advanced security and identity protection | All P1 features plus Identity Protection, Risk-based Conditional Access, Privileged Identity Management (PIM), Access reviews |
| **Microsoft Entra Suite** | ~ $12 user/month (varies) | Unified identity, access and security | Includes Entra ID P2 plus Identity Governance, Verified ID, Internet Access and Private Access (Zero Trust access solutions) |

---

## Feature Comparison
[Back to Index](#index)

| Feature | Free | P1 | P2 |
|---|---|---|---|
| User & Group Management | Yes | Yes | Yes |
| Single Sign-On (SSO) | Yes | Yes | Yes |
| Self-Service Password Reset | Basic | Advanced | Advanced |
| Conditional Access | No | Yes | Yes |
| Dynamic Groups | No | Yes | Yes |
| Hybrid Identity | Limited | Yes | Yes |
| Identity Protection | No | No | Yes |
| Privileged Identity Management (PIM) | No | No | Yes |
| Access Reviews | No | No | Yes |


## Typical Use Cases
[Back to Index](#index)

| Plan | Typical Scenario |
|---|---|
| **Free** | Small environments, basic Azure identity management |
| **P1** | Enterprises using Microsoft 365, hybrid identity, Conditional Access policies |
| **P2** | Organizations requiring advanced security, Zero Trust and privileged access management |

---

# Azure Account Types and Tenant Creation
[Back to Index](#index)

## 1. Azure Free Account
[Back to Index](#index)

Cuando creas una **Azure Free Account**, Microsoft **crea automáticamente un Microsoft Entra ID tenant** si no tenías uno previamente.

### Flujo de creación

1. Te registras con tu **Microsoft account** (por ejemplo `tuemail@gmail.com`).
2. Microsoft crea automáticamente un **Microsoft Entra ID tenant** para representar tu organización.
3. Ese tenant recibe un dominio inicial del tipo: ``<nombre>.onmicrosoft.com``
4. 4. Dentro de ese tenant se crea tu **Azure Free Subscription**.

### Estructura
````
Microsoft Entra ID Tenant
(nombre-aleatorio.onmicrosoft.com)
│
└── Azure Free Subscription
│
└── Resource Groups
└── Azure Resources
````
### Qué tenant se crea

Normalmente aparecerá como: 
````
Default Directory
xxxx.onmicrosoft.com
````

## 2. Pay-As-You-Go Account
[Back to Index](#index)

Una **Pay-As-You-Go subscription** puede crearse de dos maneras:

- dentro de un **tenant existente**
- creando **un tenant nuevo automáticamente** durante el registro

### Escenario 1 — Ya tienes tenant

Si ya tienes un tenant (por ejemplo de Microsoft 365 o una Free Account):

1. Creas una **Pay-As-You-Go subscription**
2. La subscription se **asocia al tenant existente**

### Estructura
````
Microsoft Entra ID Tenant
(contoso.onmicrosoft.com)
│
├── Free Subscription
│
└── Pay-As-You-Go Subscription
│
└── Resource Groups
└── Azure Resources
````

### Escenario 2 — No tienes tenant

Si es la **primera vez que usas Azure**:

1. Te registras
2. Microsoft crea automáticamente:

- un **Microsoft Entra ID tenant**
- una **Pay-As-You-Go subscription**

### Estructura
````
Microsoft Entra ID Tenant
(contoso.onmicrosoft.com)
│
└── Pay-As-You-Go Subscription
│
└── Resource Groups
└── Azure Resources
````

## Resumen
[Back to Index](#index)

| Account Type | Tenant Creation | Subscription |
|---|---|---|
| **Azure Free Account** | Microsoft crea automáticamente un tenant si no existe | Free Subscription dentro del tenant |
| **Pay-As-You-Go** | Puede usar un tenant existente o crear uno nuevo automáticamente | Pay-As-You-Go Subscription |

## Regla importante

````
Tenant → gestiona identidades
Subscription → gestiona facturación y recursos
````
- Un **tenant puede tener muchas subscriptions**
- Una **subscription pertenece a un solo tenant**

---

# Crear una Infraestructura Azure desde Cero (Empresa)
[Back to Index](#index)

## Orden recomendado
[Back to Index](#index)

1. Crear el **Microsoft Entra ID Tenant**
2. Definir el **dominio corporativo**
3. Crear las **suscripciones**
4. Diseñar la **jerarquía de Management Groups**
5. Aplicar **RBAC y políticas**
6. Diseñar **red y seguridad**
7. Crear **Resource Groups**
8. Desplegar **recursos**

---

## 1. Crear el Microsoft Entra ID Tenant
[Back to Index](#index)

El **tenant es la raíz de identidad de la organización en Microsoft Cloud**.

Aquí vivirán:

- usuarios
- grupos
- aplicaciones
- políticas de identidad

Dominio inicial: ``empresa.onmicrosoft.com``

Después normalmente se añade el dominio corporativo: ``empresa.com``

## 2. Añadir el dominio corporativo
[Back to Index](#index)

En **Microsoft Entra ID → Custom Domains**

Ejemplo: ``empresa.com``
Esto permite que los usuarios sean: ``usuario@empresa.com``

## 3. Crear las suscripciones
[Back to Index](#index)

Las **suscripciones son contenedores de facturación y recursos**.

Ejemplo típico empresarial:
````
Tenant
│
├── Platform Subscription
├── Production Subscription
├── NonProduction Subscription
└── Sandbox Subscription
````

## 4. Crear la jerarquía de Management Groups
[Back to Index](#index)

Esto permite gobernanza centralizada.

Ejemplo:
````
Tenant Root
│
├── Platform
│ ├── Identity
│ ├── Management
│ └── Connectivity
│
└── Landing Zones
├── Production
└── NonProduction
````

## 5. Configurar RBAC
[Back to Index](#index)

Asignar roles:

- Global Administrator
- Subscription Owner
- Contributor
- Reader

Principio: ``least privilege``

## 6. Aplicar Azure Policies
[Back to Index](#index)

Ejemplos:

- obligar tags
- restringir regiones
- exigir private endpoints
- exigir encryption

---

## 7. Diseñar la red
[Back to Index](#index)

Normalmente: 
````
Hub-Spoke
Hub VNet
│
├── Firewall
├── VPN / ExpressRoute
└── Shared services

Spoke VNets
│
├── App1
├── App2
└── App3

````
## 8. Crear Resource Groups
[Back to Index](#index)

Agrupan recursos relacionados.

Ejemplo:

````
rg-app1-prod
rg-app1-dev
rg-network
rg-monitoring
````
## 9. Desplegar recursos
[Back to Index](#index)

Ejemplos:

- VMs
- App Services
- Azure SQL
- Storage
- Key Vault

---

# ¿Para crear el tenant tengo que crear primero la suscripción?
[Back to Index](#index)

**No necesariamente**, pero **cuando te registras en Azure normalmente ocurre esto automáticamente**:

1. Creas cuenta Azure
2. Microsoft crea Tenant
3. Microsoft crea primera Subscription
````
# Estructura final típica
Tenant (Microsoft Entra ID)
│
├── Management Groups
│
├── Subscriptions
│ ├── Platform
│ ├── Production
│ ├── NonProduction
│
└── Resource Groups
└── Resources
````

--- 

# Orden Real para Crear un Entorno Azure desde Cero - Basado en Microsoft Cloud Adoption Framework
[Back to Index](#index)

Microsoft no empieza desplegando recursos.  
Primero se define **identidad, gobernanza y plataforma**.

El orden recomendado es el siguiente.

## 1. Crear el Microsoft Entra ID Tenant
[Back to Index](#index)

Es la **raíz de identidad de toda la organización en Microsoft Cloud**.

Aquí vivirán:

- usuarios
- grupos
- aplicaciones
- RBAC
- políticas de identidad

Dominio inicial:

```
empresa.onmicrosoft.com
```

Después normalmente se añade el dominio corporativo:

```
empresa.com
```

## 2. Asegurar la identidad
[Back to Index](#index)

Antes de crear infraestructura se protegen las identidades.

Configurar:

- MFA obligatorio
- Conditional Access
- Break Glass accounts
- Privileged Identity Management (PIM)

## 3. Crear las primeras Suscripciones
[Back to Index](#index)

Las suscripciones separan:

- facturación
- seguridad
- entornos

Ejemplo típico:

```
Tenant
│
├── Platform
├── Production
├── NonProduction
└── Sandbox
```

## 4. Crear la jerarquía de Management Groups
[Back to Index](#index)

Sirve para aplicar gobernanza centralizada.

Ejemplo recomendado por Microsoft (Landing Zone):

```
Tenant Root
│
├── Platform
│   ├── Identity
│   ├── Management
│   └── Connectivity
│
└── Landing Zones
    ├── Production
    └── NonProduction
```

## 5. Crear las suscripciones de plataforma
[Back to Index](#index)

Microsoft recomienda **3 suscripciones base**.

```
Platform
│
├── Identity
├── Management
└── Connectivity
```

| Subscription | Propósito |
|---|---|
| Identity | AD DS, AAD DS, identity services |
| Management | monitoring, logging, automation |
| Connectivity | redes compartidas, firewall |

## 6. Diseñar la red base
[Back to Index](#index)

Normalmente arquitectura **Hub-Spoke**.

```
Hub VNet
│
├── Azure Firewall
├── VPN Gateway / ExpressRoute
├── Bastion
└── Shared Services

Spoke VNets
│
├── App1
├── App2
└── App3
```


## 7. Aplicar Gobernanza
[Back to Index](#index)

Antes de desplegar workloads se aplican:

### RBAC

| Rol | Uso |
|---|---|
| Owner | administración completa |
| Contributor | gestión de recursos |
| Reader | solo lectura |

### Azure Policies

Ejemplos:

- obligar **tags**
- restringir **regiones**
- exigir **private endpoints**
- exigir **encryption**

## 8. Configurar Observabilidad
[Back to Index](#index)

Centralizar logs y métricas.

Servicios típicos:

- Log Analytics
- Azure Monitor
- Defender for Cloud
- Alerts

## 9. Crear las Landing Zones de Aplicación
[Back to Index](#index)

Ahora sí se crean las suscripciones donde vivirán las aplicaciones.

```
Landing Zones
│
├── Production
│   ├── Subscription App1
│   └── Subscription App2
│
└── NonProduction
    ├── Subscription Dev
    └── Subscription Test
```

## 10. Desplegar Workloads
[Back to Index](#index)

Dentro de las suscripciones:

```
Subscription
│
├── Resource Groups
│   ├── rg-app1-prod
│   ├── rg-network
│   └── rg-monitoring
│
└── Resources
    ├── VMs
    ├── Databases
    ├── Storage
    └── KeyVault
```


## Jerarquía final completa de Azure
[Back to Index](#index)

```
Tenant
│
├── Management Groups
│
├── Subscriptions
│
├── Resource Groups
│
└── Resources
```


## Regla importante
[Back to Index](#index)

| Nivel | Función |
|---|---|
| Tenant | Identidad |
| Management Group | Gobernanza |
| Subscription | Facturación y límite administrativo |
| Resource Group | Organización de recursos |
| Resource | Servicio Azure |


## Error típico cuando se empieza con Azure
[Back to Index](#index)

Mucha gente empieza así:

```
Subscription → VM
```

Pero la arquitectura correcta es:

```
Tenant
→ Management Groups
→ Subscriptions
→ Governance
→ Platform
→ Workloads
```

---

# patrón recomendado por Microsoft

Las subscriptions no deberían pertenecer a personas, sino a grupos o cuentas de plataforma.

Arquitectura recomendada:
````
Tenant
│
├── Entra ID Group
│      AZ-Platform-Subscription-Owners
│
└── Subscription
       Owner → AZ-Subscription-Owners
````

# Cómo arreglar las subscriptions actuales
## Paso 1 — crear grupo de administradores

En Microsoft Entra ID → Groups

- Crear algo como: ``AZ-Platform-Subscription-Owners``
- Tipo: ``Security Group``

Añadir:

- cloud architects
- platform team
- admins reales

## Paso 2 — asignar el grupo como Owner

En cada subscription:

````
Subscriptions
→ Access Control (IAM)
→ Add role assignment
````

Asignar:

````
Role: Owner
Member: AZ-Platform-Subscription-Owners
````

## Paso 3 — eliminar al usuario antiguo

Una vez el grupo tenga permisos:
````
Remove role assignment
frank.sinatra@nyRZV.onmicrosoft.com
``
