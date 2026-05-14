[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# EntraID 

# Índice

- [Microsoft Active Directory (On-Premises) vs Microsoft Entra ID](#microsoft-active-directory-on-premises-vs-microsoft-entra-id)
- [Active Directory vs Microsoft Entra ID](#active-directory-vs-microsoft-entra-id)
- [Microsoft Entra ID Plans and Pricing](#microsoft-entra-id-plans-and-pricing)
- [Azure Account Types and Tenant Creation](#azure-account-types-and-tenant-creation)
- [Crear una Infraestructura Azure desde Cero (Empresa)](#crear-una-infraestructura-azure-desde-cero-empresa)
- [¿Para crear el tenant tengo que crear primero la suscripción?](#para-crear-el-tenant-tengo-que-crear-primero-la-suscripción)
- [Orden Real para Crear un Entorno Azure desde Cero - Basado en Microsoft Cloud Adoption Framework](#orden-real-para-crear-un-entorno-azure-desde-cero---basado-en-microsoft-cloud-adoption-framework)
- [patrón recomendado por Microsoft](#patrón-recomendado-por-microsoft)
- [Cómo arreglar las subscriptions actuales](#cómo-arreglar-las-subscriptions-actuales)
- [Microsoft Entra ID - Group Naming Policy](#microsoft-entra-id---group-naming-policy)
- [Microsoft Entra ID - Security Groups (AZ-104)](#microsoft-entra-id---security-groups-az-104)
- [Microsoft Fabric](#microsoft-fabric)
- [Microsoft Fabric Licensing y Group-Based Licensing](#microsoft-fabric-licensing-y-group-based-licensing)
- [Self-Service Password Reset (SSPR) en Microsoft Entra ID](#self-service-password-reset-sspr-en-microsoft-entra-id)
- [Microsoft Entra Custom Security Attributes (AZ-104)](#microsoft-entra-custom-security-attributes-az-104)

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
````

# Microsoft Entra ID - Group Naming Policy

## Qué es una Group Naming Policy

Una Group Naming Policy permite controlar automáticamente el nombre de los grupos Microsoft 365 creados en Microsoft Entra ID.

---

## Objetivos típicos

Permite:

- Agregar prefijos
- Agregar sufijos
- Usar atributos dinámicos
- Bloquear palabras prohibidas
- Estandarizar nombres

---

## Ejemplo típico

Formato requerido:

```text
<Department><GroupName>
```

Resultado:

```text
IT-FinanceTeam
HR-Recruitment
Sales-Europe
```

---

## Dónde se configura

En:

```text
Microsoft Entra Admin Center
    ↓
Groups
    ↓
Naming policy
```

---

## Componentes importantes

| Elemento | Función |
|---|---|
| Prefix | Añadir texto/atributo al inicio |
| Suffix | Añadir texto/atributo al final |
| String | Texto fijo |
| Attribute | Valor dinámico del usuario |

---

## Prefix vs Suffix

| Tipo | Ejemplo |
|---|---|
| Prefix | IT-TeamA |
| Suffix | TeamA-IT |

---

## String vs Attribute

### String

Texto fijo/manual.

Ejemplo:

```text
CORP-TeamA
```

Siempre igual.

---

### Attribute

Usa un atributo del usuario de Entra ID.

Ejemplo:

```text
IT-TeamA
HR-TeamA
Finance-TeamA
```

Depende del atributo del creador del grupo.

---

## Atributos soportados típicos

| Atributo | Ejemplo |
|---|---|
| Department | IT |
| Company | Contoso |
| Office | Barcelona |
| Country | Spain |

---

## Escenario típico examen

Requisito:

```text
<Department><GroupName>
```

---

## Configuración correcta

### Paso 1

Crear:

```text
Group Naming Policy
```

---

### Paso 2

Seleccionar:

```text
Add prefix → Attribute
```

---

### Paso 3

Elegir atributo:

```text
Department
```

---

## Resultado

Si el usuario pertenece a:

```text
Department = IT
```

y crea:

```text
ProjectTeam
```

el resultado será:

```text
IT-ProjectTeam
```

---

## Diferencia importante

| Configuración | Resultado |
|---|---|
| Prefix + String | Texto fijo |
| Prefix + Attribute | Valor dinámico |
| Suffix + Attribute | Valor dinámico al final |

---

## Ejemplos

### Prefix + String

```text
CORP-TeamA
```

---

### Prefix + Attribute

```text
IT-TeamA
```

---

### Suffix + Attribute

```text
TeamA-IT
```

---

## Palabras bloqueadas

Las naming policies también permiten bloquear palabras como:

- CEO
- Admin
- Root

---

## Ejemplo

```text
CEO-Team
```

❌ Bloqueado.

---

## Qué NO hace

❌ No cambia grupos existentes  
❌ No modifica automáticamente grupos antiguos  
❌ No aplica a todos los tipos grupos Azure  

---

## Tipos de grupos afectados

Principalmente:

✅ Microsoft 365 Groups  
✅ Teams (porque usan M365 Groups)

---

## Trampa típica AZ-104

Muchos candidatos confunden:

```text
Attribute
```

con:

```text
String
```

---

## Regla importante

```text
Attribute = valor dinámico del usuario
```

```text
String = texto fijo
```

---

## Diferencia importante examen

| Necesidad | Configuración |
|---|---|
| Prefijo fijo | Prefix + String |
| Prefijo dinámico | Prefix + Attribute |
| Sufijo dinámico | Suffix + Attribute |

---

## Qué quiere evaluar Microsoft

| Concepto | Importancia |
|---|---|
| Naming Policies | Alta |
| Prefix vs Suffix | Alta |
| String vs Attribute | Muy alta |
| Department attribute | Alta |

---

## Tabla resumen examen

| Requisito | Configuración correcta |
|---|---|
| IT-TeamA | Prefix + Attribute(Department) |
| CORP-TeamA | Prefix + String |
| TeamA-IT | Suffix + Attribute(Department) |

---

## Regla rápida examen

```text
Use Attribute when the value must come dynamically from Entra ID.
```

---

## Frases clave AZ-104

```text
Group naming policies can use Entra ID user attributes.
```

```text
Department can be used as a dynamic prefix or suffix.
```

```text
String values are static, attributes are dynamic.
```

---

# Microsoft Entra ID - Security Groups (AZ-104)

## Qué es un Security Group

Un:

```text
Security Group
```

es un grupo utilizado para:

- agrupar usuarios o dispositivos
- asignar permisos RBAC
- aplicar políticas de seguridad
- simplificar la administración de accesos

---

## Idea principal

En lugar de asignar permisos usuario por usuario:

❌ Incorrecto

```text
User1 → Contributor
User2 → Contributor
User3 → Contributor
```

se hace:

✅ Correcto

```text
Security Group
    ↓
Contributor
```

y dentro del grupo:

```text
User1
User2
User3
```

Todos heredan permisos automáticamente.

---

## Ejemplo visual

```text
Security Group: AZ-VM-Admins
        ↓
Role: Virtual Machine Contributor
        ↓
Subscription
```

Usuarios dentro del grupo:

```text
Alice
Bob
Charlie
```

---

## Para qué sirve

| Uso | Ejemplo |
|---|---|
| RBAC | Contributor sobre subscription |
| Conditional Access | MFA obligatorio |
| Intune | Políticas dispositivos |
| Microsoft 365 | Permisos aplicaciones |
| Azure Policies | Scope dinámico |

---

## Tipos de grupos en Entra ID

| Tipo grupo | Uso principal |
|---|---|
| Security Group | Seguridad y permisos |
| Microsoft 365 Group | Colaboración |
| Distribution List | Correo |
| Mail-enabled Security Group | Seguridad + email |

---

## Security Group

### Características

| Feature | Soportado |
|---|---|
| RBAC | ✅ |
| Conditional Access | ✅ |
| Usuarios | ✅ |
| Devices | ✅ |
| Dynamic Membership | ✅ |
| Email/Teams | ❌ normalmente |

---

## Microsoft 365 Group

Pensado principalmente para colaboración.

Incluye:

- Teams
- SharePoint
- Outlook
- Planner

---

## Diferencia importante

| Grupo | Objetivo |
|---|---|
| Security Group | Seguridad y permisos |
| Microsoft 365 Group | Colaboración |

---

## Dynamic Groups

Un Security Group puede ser:

| Tipo | Explicación |
|---|---|
| Assigned | Miembros manuales |
| Dynamic User | Usuarios automáticos |
| Dynamic Device | Dispositivos automáticos |

---

## Ejemplo Dynamic Group

```text
Department = IT
```

Usuarios IT entran automáticamente.

---

## Uso MUY importante en Azure

Microsoft recomienda:

✅ asignar RBAC a grupos  
❌ NO asignar RBAC a usuarios individuales  

---

## Buen patrón Azure

```text
Security Group
    ↓
RBAC Role
    ↓
Subscription / RG / Resource
```

---

## Ejemplos típicos empresa

```text
AZ-Subscription-Owners
AZ-Network-Contributors
AZ-VM-Admins
AZ-Readers
```

---

## Relación con RBAC

Los grupos se usan muchísimo para:

| Rol RBAC | Ejemplo grupo |
|---|---|
| Owner | AZ-Subscription-Owners |
| Contributor | AZ-App-Contributors |
| Reader | AZ-Readers |

---

## Ventajas

| Ventaja | Explicación |
|---|---|
| Escalable | Fácil añadir usuarios |
| Menos errores | RBAC centralizado |
| Mejor auditoría | Permisos agrupados |
| Fácil automatización | Terraform/Bicep |

---

## Trampa típica AZ-104

Muchos candidatos creen:

```text
Security Group = grupo de correo
```

❌ Incorrecto.

Su objetivo principal es:

```text
seguridad y control de acceso
```

---

## Regla rápida examen

```text
RBAC should normally be assigned to groups, not individual users.
```

---

## Qué quiere evaluar Microsoft

| Concepto | Importancia |
|---|---|
| Security Groups | Muy alta |
| RBAC mediante grupos | Muy alta |
| Dynamic Groups | Alta |
| Group-based access | Muy alta |

---

## Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Gestionar permisos Azure | Security Group |
| Teams / colaboración | Microsoft 365 Group |
| Asignar RBAC | Security Group |
| Dynamic membership | Dynamic Group |

---

## Frases clave AZ-104

```text
Security groups are commonly used to assign RBAC permissions.
```

```text
Security groups simplify permission management.
```

```text
Dynamic groups can automatically include users based on attributes.
```
---
# Microsoft Fabric

## Qué significa “You purchase a Microsoft Fabric license”

Significa que una organización compra una licencia o capacidad de Microsoft Fabric para poder utilizar sus servicios y funcionalidades.

---

## Qué es Microsoft Fabric

Microsoft Fabric es una plataforma SaaS de análisis de datos unificada que integra:

- Data Engineering
- Data Factory
- Data Warehouse
- Data Science
- Real-Time Analytics
- Power BI

todo dentro de una misma plataforma.

---

## Idea principal

Microsoft intenta unificar:

```text
Data Lake
ETL
Analytics
BI
AI
Streaming
```

en un único servicio cloud.

---

## Arquitectura conceptual

```text
Microsoft Fabric
    ↓
OneLake
    ↓
Workloads
        ├─ Power BI
        ├─ Data Factory
        ├─ Data Engineering
        ├─ Data Warehouse
        └─ Data Science
```

---

## OneLake

### Qué es

Es el data lake central de Fabric.

Microsoft lo presenta como:

```text
OneDrive for data
```

---

## Qué incluye Microsoft Fabric

| Servicio | Función |
|---|---|
| Power BI | Reporting y dashboards |
| Data Factory | ETL / pipelines |
| Data Engineering | Spark / notebooks |
| Data Warehouse | SQL analytics |
| Data Science | Machine Learning |
| Real-Time Analytics | Streaming y logs |

---

## Qué significa “license”

En Fabric normalmente “license” puede referirse a:

| Tipo | Explicación |
|---|---|
| User License | Licencia usuario |
| Capacity License | Capacidad compute dedicada |

---

## Tipos típicos

| Tipo | Uso |
|---|---|
| Free | Funcionalidad limitada |
| Pro | Power BI Pro |
| Premium Per User (PPU) | Funciones premium usuario |
| Fabric Capacity (F SKU) | Compute compartido Fabric |

---

## Importante

Fabric usa capacidades llamadas:

```text
F SKUs
```

Ejemplo:

| SKU | Tamaño |
|---|---|
| F2 | Pequeño |
| F64 | Grande |

---

## Qué habilita la licencia

Dependiendo del tipo:

✅ Crear workspaces  
✅ Ejecutar pipelines  
✅ Crear warehouses  
✅ Ejecutar notebooks Spark  
✅ Compartir Power BI  
✅ Usar OneLake  

---

## Relación con Power BI

Fabric está muy integrado con Power BI.

De hecho:

```text
Power BI es parte de Microsoft Fabric
```

---

## SaaS

Fabric es un servicio:

```text
SaaS fully managed
```

No administras:

- servidores
- clusters
- infraestructura

Microsoft administra todo.

---

## Trampa típica

Muchos creen:

```text
Fabric = solo Power BI
```

❌ Incorrecto.

Power BI es solo una parte.

---

## Qué quiere evaluar Microsoft

Cuando una pregunta dice:

```text
You purchase a Microsoft Fabric license
```

normalmente quiere evaluar:

- capacidades habilitadas
- licenciamiento
- integración Power BI/Fabric
- gobernanza datos
- capacidades Premium

---

## Frase rápida

```text
Microsoft Fabric is Microsoft's unified SaaS analytics platform.
```
---
# Microsoft Fabric Licensing y Group-Based Licensing

## Escenario

El tenant tiene:

| Elemento | Valor |
|---|---|
| Tenant | Default Directory |
| Tipo licencia Entra ID | Free |
| Dominio | onmicrosoft.com |

---

## Identidades existentes

| Identidad | Tipo |
|---|---|
| UserA | Usuario |
| SecGroup01 | Security Group |
| M365Group01 | Microsoft 365 Group |

---

## Acción realizada

```text
You purchase a Microsoft Fabric license
```

---

## Pregunta

¿Qué identidades pueden recibir la licencia?

---

## Respuesta correcta

✅ Solo:

```text
UserA
```

---

## Concepto clave

En Microsoft Entra ID Free:

❌ NO existe Group-Based Licensing

---

## Qué es Group-Based Licensing

Permite asignar licencias automáticamente a grupos.

Ejemplo:

```text
Security Group
    ↓
Microsoft Fabric License
    ↓
Todos los usuarios heredan la licencia
```

---

## Importante

Esta funcionalidad requiere:

| Licencia Entra ID | Soportado |
|---|---|
| Free | ❌ |
| P1 | ✅ |
| P2 | ✅ |

---

## Qué ocurre en Entra ID Free

Solo puedes asignar licencias:

✅ directamente a usuarios

---

## Qué NO puedes hacer

❌ Asignar licencias a Security Groups  
❌ Asignar licencias a Microsoft 365 Groups  
❌ Group-based licensing  

---

## Por qué UserA sí funciona

Porque:

```text
las licencias pueden asignarse directamente a usuarios individuales
```

---

## Por qué SecGroup01 NO funciona

Aunque sea un:

```text
Security Group
```

Entra ID Free:

❌ no soporta group-based licensing

---

## Por qué M365Group01 NO funciona

Aunque sea un:

```text
Microsoft 365 Group
```

sigue siendo:

```text
group licensing
```

y requiere:

✅ Entra ID P1 o P2

---

## Trampa típica examen

Muchos candidatos piensan:

```text
si existe el grupo → puedo asignar licencias
```

❌ Incorrecto.

La clave NO es el grupo.

La clave es:

```text
el tipo de licencia Entra ID
```

---

## Diferencia importante

| Feature | Entra ID Free | Entra ID P1/P2 |
|---|---|---|
| Licencias usuario manual | ✅ | ✅ |
| Group-based licensing | ❌ | ✅ |
| Licencias a Security Groups | ❌ | ✅ |
| Licencias a M365 Groups | ❌ | ✅ |

---

## Flujo permitido en Free

```text
Admin
    ↓
Assign license
    ↓
UserA
```

---

## Flujo NO permitido en Free

```text
Admin
    ↓
Assign license
    ↓
Security Group
    ↓
Users
```

❌ No soportado.

---

## Qué quiere evaluar Microsoft

| Concepto | Importancia |
|---|---|
| Entra ID licensing | Alta |
| Group-based licensing | Muy alta |
| Free vs P1/P2 | Muy alta |
| Security Groups | Alta |

---

## Regla rápida examen

```text
Group-based licensing requires Microsoft Entra ID P1 or P2.
```

---

## Tabla resumen examen

| Identidad | Puede recibir licencia en Entra Free |
|---|---|
| Usuario | ✅ |
| Security Group | ❌ |
| Microsoft 365 Group | ❌ |

---

## Frases clave AZ-104

```text
Microsoft Entra ID Free does not support group-based licensing.
```

```text
Licenses can always be assigned directly to users.
```

```text
Group-based licensing requires Entra ID P1 or P2.
```
---
# Self-Service Password Reset (SSPR) en Microsoft Entra ID

## Escenario

Existen las siguientes identidades:

| Identidad | Tipo |
|---|---|
| UserA | Usuario |
| SecGroup01 | Security Group |
| M365Group01 | Microsoft 365 Group |

---

## Pregunta

¿Para qué identidades puede habilitarse SSPR?

---

## Respuesta correcta

✅

```text
SecGroup01 and M365Group01 only
```

---

# Qué es SSPR

SSPR significa:

```text
Self-Service Password Reset
```

Permite que los usuarios:

- cambien su password
- recuperen acceso
- reseteen contraseña

sin intervención del administrador.

---

# Cómo se habilita SSPR

En Microsoft Entra ID:

```text
Microsoft Entra ID
    ↓
Password reset
    ↓
Properties
```

---

# Opciones disponibles

| Configuración | Soportado |
|---|---|
| All users | ✅ |
| Selected groups | ✅ |
| Usuario individual | ❌ |

---

# Concepto clave examen

SSPR:

❌ NO se asigna directamente a usuarios individuales  
✅ se asigna a grupos o a todos los usuarios  

---

# Por qué UserA NO es correcto

Aunque UserA es un usuario válido:

❌ SSPR no puede habilitarse directamente sobre un usuario individual.

---

# Por qué SecGroup01 SÍ es correcto

Porque SSPR puede habilitarse sobre:

```text
Security Groups
```

---

# Por qué M365Group01 SÍ es correcto

Porque SSPR también soporta:

```text
Microsoft 365 Groups
```

---

# Cómo funciona realmente

Ejemplo:

```text
Security Group
    ↓
SSPR enabled
    ↓
Todos los usuarios del grupo
```

---

# Ejemplo práctico

## Grupo

```text
IT-Users
```

---

## Configuración

```text
Enable SSPR for selected groups
```

---

## Resultado

Todos los miembros de:

```text
IT-Users
```

pueden usar SSPR.

---

# Importante

El grupo NO resetea passwords.

Los usuarios dentro del grupo son los que reciben la capacidad SSPR.

---

# Diferencia importante

| Objeto | Puede recibir SSPR directamente |
|---|---|
| Usuario individual | ❌ |
| Security Group | ✅ |
| Microsoft 365 Group | ✅ |

---

# Trampa típica examen

Muchos candidatos piensan:

```text
SSPR se habilita usuario por usuario
```

❌ Incorrecto.

Microsoft lo diseña para:

✅ grupos  
✅ todos los usuarios  

---

# Otra trampa típica

Muchos creen:

```text
solo Security Groups sirven
```

❌ Incorrecto.

También funcionan:

✅ Microsoft 365 Groups

---

# Qué quiere evaluar Microsoft

| Concepto | Importancia |
|---|---|
| SSPR | Muy alta |
| Group-based configuration | Muy alta |
| Security Groups | Alta |
| M365 Groups | Alta |

---

# Tabla resumen examen

| Configuración SSPR | Soportado |
|---|---|
| Usuario individual | ❌ |
| Security Group | ✅ |
| Microsoft 365 Group | ✅ |
| Todos los usuarios | ✅ |

---

# Regla rápida examen

```text
SSPR is enabled for groups or all users, not for individual users.
```

---

# Frases clave AZ-104

```text
Self-Service Password Reset can be enabled for selected groups.
```

```text
Microsoft 365 Groups can be used for SSPR scope.
```

```text
SSPR is not assigned directly to individual users.
```


---

# Microsoft Entra Custom Security Attributes (AZ-104)

## Índice

- [Microsoft Entra Custom Security Attributes (AZ-104)](#microsoft-entra-custom-security-attributes-az-104)
- [Qué son los Custom Security Attributes](#qué-son-los-custom-security-attributes)
- [Para qué sirven](#para-qué-sirven)
- [Qué objetos soportan](#qué-objetos-soportan)
- [Qué objetos NO soportan](#qué-objetos-no-soportan)
- [Por qué la respuesta es False](#por-qué-la-respuesta-es-false)
- [Roles necesarios](#roles-necesarios)
- [Importante sobre Global Administrator](#importante-sobre-global-administrator)
- [Tipos de roles relacionados](#tipos-de-roles-relacionados)
- [Ejemplo conceptual](#ejemplo-conceptual)
- [Casos típicos de uso](#casos-típicos-de-uso)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué son los Custom Security Attributes

Son atributos personalizados de Microsoft Entra ID.

Permiten añadir metadata de seguridad a identidades.

---

# Para qué sirven

Permiten:

- clasificación identidades
- políticas acceso
- automatización
- Zero Trust
- RBAC avanzado
- Conditional Access avanzado

---

# Ejemplos

| Atributo | Valor |
|---|---|
| Department | Finance |
| ClearanceLevel | High |
| DataSensitivity | Confidential |

---

# Qué objetos soportan

Los Custom Security Attributes pueden asignarse a:

| Objeto | Soportado |
|---|---|
| Users | ✅ |
| Service Principals | ✅ |
| Managed Identities | ✅ |

---

# Qué objetos NO soportan

| Objeto | Soportado |
|---|---|
| Groups | ❌ |
| Devices | ❌ |
| Subscriptions | ❌ |

---

# Por qué la respuesta es False

La pregunta dice:

```text
Admin1 can assign Attribute1 to Group1
```

❌ Incorrecto.

Porque:

```text
Custom Security Attributes NO pueden asignarse a grupos
```

---

# Importante

Aunque Admin1 fuera:

```text
Global Administrator
```

seguiría siendo:

❌ imposible  

porque el problema NO es el permiso.

El problema es:

```text
el tipo de objeto soportado
```

---

# Roles necesarios

Microsoft Entra utiliza roles específicos para Custom Security Attributes.

---

# Importante sobre Global Administrator

Global Administrator:

❌ NO recibe automáticamente permisos completos sobre Custom Security Attributes.

---

# Tipos de roles relacionados

| Rol | Función |
|---|---|
| Attribute Definition Administrator | Crear atributos |
| Attribute Assignment Administrator | Asignar atributos |
| Attribute Assignment Reader | Leer atributos |

---

# Ejemplo conceptual

```text
User
   ↓
Custom Security Attribute
   ↓
Classification / Policies
```

---

# Casos típicos de uso

| Caso | Ejemplo |
|---|---|
| Clasificación usuarios | Confidential |
| Automatización acceso | Region=EU |
| Conditional Access | SecurityLevel=High |
| Zero Trust | Risk categories |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
Groups soportan Custom Security Attributes
```

❌ Incorrecto.

---

## Trampa 2

Pensar que:

```text
Global Administrator puede hacer cualquier cosa
```

❌ Incorrecto.

Muchos servicios Entra usan:

```text
roles especializados
```

---

## Trampa 3

Confundir:

```text
Custom Security Attributes
```

con:

```text
atributos normales usuario
```

---

# Diferencia importante

| Tipo | Uso |
|---|---|
| User attributes | Información usuario |
| Custom Security Attributes | Clasificación seguridad |

---

# Tabla resumen examen

| Objeto | Soporta Custom Security Attributes |
|---|---|
| User | ✅ |
| Service Principal | ✅ |
| Managed Identity | ✅ |
| Group | ❌ |

---

# Reglas rápidas AZ-104

```text
Custom Security Attributes cannot be assigned to groups.
```

```text
Global Administrator does not automatically manage custom security attributes.
```

```text
Specialized Entra roles are required for custom security attributes.
```

---

# Frases clave AZ-104

```text
Custom Security Attributes support users, service principals, and managed identities.
```

```text
Groups are not supported assignment targets.
```

```text
Attribute Assignment Administrator can assign custom security attributes.
```
