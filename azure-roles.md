
[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [📚 Roles en Azure](#-roles-en-azure)
- [User Administrator vs Global Administrator vs Authentication Administrator vs User Access Administrator (AZ-104)](#user-administrator-vs-global-administrator-vs-authentication-administrator-vs-user-access-administrator-az-104)
- [Los cuatro planos principales de Azure](#los-cuatro-planos-principales-de-azure)

  
# 📚 roles en Azure

## Planes





| Sistema                       | Qué controla                                  | Rol “admin total” recomendado                     | Notas clave                                                                 |
|------------------------------|-----------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------|
| **Entra ID (Identity)**      | Identidades, autenticación, PIM               | Global Administrator                             | Máximo privilegio en identidad. Gestiona usuarios, grupos, roles y PIM      |
| **RBAC (Control plane)**     | Recursos Azure (ARM)                          | Owner (a nivel MG o Subscription)                | Control total de recursos + puede asignar roles                             |
| **Data Plane**               | Acceso a datos (KV, Storage, SQL, etc.)       | Depende del servicio (ej: Key Vault Administrator, Storage Blob Data Owner) | No hay rol único global. Se asigna por servicio                             |
| **Billing (Cost plane)**     | Costes, facturación, usage, invoices          | Billing Account Owner (o Billing Profile Owner)  | Control total de facturación. Separado completamente de RBAC                |

##  Identity Plane (EntraId)

| Scope / Plano        | Rol                          | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|---------------------|------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Identity (Entra ID) | Global Administrator          | Control total del tenant (usuarios, grupos, roles, PIM, configuración)        | Administración central de identidad                                     | Equipo de seguridad gestionando todo el acceso corporativo                              |
| Identity (Entra ID) | Privileged Role Administrator | Gestión de roles privilegiados y PIM                                          | Control de accesos críticos                                             | Equipo IAM asignando roles elevados bajo aprobación                                     |
| Identity (Entra ID) | User Administrator            | Gestión de usuarios y grupos                                                  | Operación diaria de identidades                                         | IT creando usuarios y asignándolos a grupos                                              |
| Identity (Entra ID) | Global Reader                 | Acceso de solo lectura a toda la configuración                                | Auditoría / troubleshooting                                             | Auditor revisando configuración sin modificar                                            |

##  Control Plane (RBAC)
| Scope / Plano         | Rol                      | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|----------------------|--------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| RBAC (Control Plane) | Owner                    | Control total de recursos + asignación de roles                               | Administración completa de subscriptions / MG                          | Arquitecto cloud gestionando recursos y accesos                                          |
| RBAC (Control Plane) | Contributor              | Gestión de recursos sin poder asignar permisos                                | Operación de infraestructura                                            | Ingeniero desplegando recursos                                                           |
| RBAC (Control Plane) | Reader                   | Acceso de solo lectura a recursos                                             | Observabilidad / auditoría                                              | Equipo viendo configuración sin modificar                                                |
| RBAC (Control Plane) | User Access Administrator| Gestión de permisos RBAC                                                      | Gobierno de accesos                                                     | Equipo IAM asignando roles a usuarios o grupos                                          |

## Data Plane

| Scope / Plano | Rol                           | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|--------------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Data Plane   | Key Vault Administrator        | Control total sobre secretos, claves y certificados                           | Gestión de seguridad y secretos                                         | Equipo de seguridad gestionando secretos en Key Vault                                   |
| Data Plane   | Key Vault Secrets User         | Acceso a leer/escribir secretos                                               | Consumo de secretos por apps                                            | Aplicación accediendo a credenciales                                                     |
| Data Plane   | Storage Blob Data Owner        | Control total sobre datos en blobs                                            | Gestión de almacenamiento                                               | Equipo gestionando almacenamiento de datos                                               |
| Data Plane   | Storage Blob Data Reader       | Lectura de datos en blobs                                                     | Acceso de solo lectura                                                  | Analista leyendo datos                                                                   |
| Data Plane   | SQL DB Contributor / DB roles  | Gestión o acceso a bases de datos                                             | Administración o uso de bases de datos                                  | DBA gestionando DB o aplicación accediendo a tablas                                      |

## Cost Plane (Billing)

| Scope                | Rol                          | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|----------------------|------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Billing Account      | Billing Account Owner        | Control total del contrato de facturación. Puede gestionar perfiles y acceso | Nivel corporativo / Finanzas centrales                                 | Equipo FinOps global que gestiona toda la facturación de SYNLAB                         |
| Billing Account      | Billing Account Reader       | Solo lectura del Billing Account (visión global de costes y estructura)       | Auditoría / reporting global                                           | Auditor que necesita ver el gasto total de la compañía                                  |
| Billing Profile      | Billing Profile Owner        | Control total del perfil de facturación (facturas, pagos, acceso)             | Responsable financiero de una región o unidad                          | Responsable financiero de Alemania gestionando su facturación                           |
| Billing Profile      | Billing Profile Contributor  | Puede gestionar costes (budgets, análisis), pero no todo el control total     | FinOps / Cloud team operativo                                          | Ingeniero que crea budgets y analiza costes en UK                                        |
| Billing Profile      | Billing Profile Reader       | Solo lectura de costes dentro del perfil                                      | Reporting / equipos técnicos                                           | Equipo de plataforma que revisa costes sin poder modificarlos                           |
| Invoice Section      | Invoice Section Owner        | Control total de una sección de factura (subdivisión de costes)               | Organización por departamentos / proyectos                             | Responsable de IT dentro de Alemania con control sobre su parte del gasto               |
| Invoice Section      | Invoice Section Contributor  | Gestión operativa de costes en una sección (budgets, análisis)                | Equipos de aplicación / proyectos                                      | Equipo de una aplicación que gestiona su presupuesto y consumo                          |
| Invoice Section      | Invoice Section Reader       | Solo lectura de costes en una sección concreta                                | Visibilidad para equipos                                               | Equipo de desarrollo que solo necesita ver cuánto consume su aplicación                 |


---
# User Administrator vs Global Administrator vs Authentication Administrator vs User Access Administrator (AZ-104)

En Azure existen **dos Control Planes independientes**:

- **Microsoft Entra ID (Identity Control Plane)** → Gestiona identidades.
- **Azure Resource Manager (Azure RBAC Control Plane)** → Gestiona permisos sobre recursos Azure.

Es muy habitual confundir ambos conceptos en el AZ-104.

---

# Los dos Control Planes

```text
                    Tenant

        ┌─────────────────────────┐
        │ Microsoft Entra ID      │
        │ (Identity Control Plane)│
        └──────────┬──────────────┘
                   │
          Global Administrator
               = root aquí
                   │
───────────────────┼────────────────────
                   │
        Azure Resource Manager
          (Azure RBAC Control Plane)
                   │
        Management Groups
              │
        Subscriptions
              │
        Resource Groups
              │
          Resources
```
---


# User Administrator vs Global Administrator vs Authentication Administrator vs User Access Administrator (AZ-104)

En Azure existen **cuatro planos principales**, aunque para el **AZ-104** los dos más importantes son:

- **Identity Control Plane (Microsoft Entra ID)**
- **Management Control Plane (Azure Resource Manager / Azure RBAC)**

Es muy habitual confundir los roles de administración de identidades con los de administración de recursos.

---

# Los cuatro planos principales de Azure

```text
                           Azure

        ┌─────────────────────────────────────────┐
        │ Identity Control Plane                  │
        │ Microsoft Entra ID                      │
        └─────────────────┬───────────────────────┘
                          │
                  Global Administrator
                     (root del plano)
                          │
──────────────────────────┼──────────────────────────────────
                          │
        ┌─────────────────▼───────────────────────┐
        │ Management Control Plane                │
        │ Azure Resource Manager (Azure RBAC)     │
        └─────────────────┬───────────────────────┘
                          │
                       Owner
                  (root del ámbito)
                          │
──────────────────────────┼──────────────────────────────────
                          │
        ┌─────────────────▼───────────────────────┐
        │ Data Plane                              │
        │ Datos almacenados en los recursos       │
        └─────────────────┬───────────────────────┘
                          │
            Storage Blob Data Owner
          Key Vault Administrator
               (depende del servicio)
                          │
──────────────────────────┼──────────────────────────────────
                          │
        ┌─────────────────▼───────────────────────┐
        │ Billing Plane                           │
        │ Facturación                             │
        └─────────────────┬───────────────────────┘
                          │
                 Billing Account Owner
            (depende del tipo de facturación)
```

---

# Comparativa de los cuatro planos

| Plano | ¿Qué administra? | "Root" o rol principal | Ejemplo |
|--------|------------------|------------------------|----------|
| **Identity Control Plane** | Identidades (usuarios, grupos, aplicaciones, MFA, licencias, etc.) | **Global Administrator** | Crear usuarios, configurar MFA o Conditional Access. |
| **Management Control Plane** | Recursos Azure y permisos RBAC. | **Owner** (sobre su ámbito) | Crear una VM, eliminar un Storage Account o asignar el rol Contributor. |
| **Data Plane** | Los datos almacenados dentro de los recursos. | Depende del servicio (no existe un único "root"). | Leer un blob, consultar una base de datos SQL o recuperar un secreto de Key Vault. |
| **Billing Plane** | Facturación, costes y métodos de pago. | **Billing Account Owner** (según el modelo de facturación) | Descargar facturas o consultar costes. |

---

# Comparativa de roles

| Rol | Pertenece a | ¿Es el "root" de su Control Plane? | Equivalente aproximado en Linux | ¿Qué administra? | Ejemplo |
|------|-------------|:----------------------------------:|---------------------------------|------------------|----------|
| **Global Administrator** | **Microsoft Entra ID** | ✅ Sí | **root** | Administración total del tenant de Microsoft Entra ID (usuarios, grupos, aplicaciones, licencias, Conditional Access, Identity Governance, etc.). | Crear usuarios, asignar licencias, configurar MFA, crear aplicaciones Enterprise, etc. |
| **User Administrator** | **Microsoft Entra ID** | ❌ No | Administrador de usuarios (`useradd`, `passwd`) | Gestiona usuarios y grupos. Puede crear, modificar y eliminar usuarios, restablecer contraseñas y asignar determinadas licencias. | Crear un usuario y restablecer su contraseña. |
| **Authentication Administrator** | **Microsoft Entra ID** | ❌ No | Administrador de autenticación/PAM | Gestiona los métodos de autenticación (MFA, Microsoft Authenticator, FIDO2, Windows Hello for Business, etc.). | Restablecer el registro MFA de un usuario. |
| **User Access Administrator** | **Azure RBAC** | ❌ No | Administrador de permisos (`chmod`, `chown`, `setfacl`, `sudoers`) | Administra los **Role Assignments** de Azure RBAC. Decide quién tiene acceso a los recursos Azure, pero no administra los propios recursos. | Asignar el rol **Contributor** sobre una suscripción a un usuario. |
| **Owner (Azure RBAC)** | **Azure RBAC** | ✅ Sí (sobre su ámbito) | **root** sobre una Subscription, Resource Group o Resource | Administración total de los recursos del ámbito asignado. Además, puede crear y eliminar **Role Assignments**. | Administrar una suscripción completa y asignar el rol **Reader** a otros usuarios. |

---

# 1. Global Administrator

Es el rol con más privilegios de **Microsoft Entra ID**.

Puede administrar prácticamente todo el tenant.

Ejemplos:

- Usuarios
- Grupos
- Aplicaciones
- Licencias
- Conditional Access
- Privileged Identity Management (PIM)
- Identity Governance
- Métodos de autenticación
- Configuración del tenant

> Es el equivalente más cercano al usuario **root** dentro del **Identity Control Plane**.

---

# 2. User Administrator

Gestiona identidades.

Puede:

- Crear usuarios.
- Eliminar usuarios.
- Modificar usuarios.
- Restablecer contraseñas.
- Administrar grupos.
- Asignar determinadas licencias.

No administra permisos RBAC sobre recursos Azure.

> Equivalente aproximado: **`useradd`**, **`passwd`**.

---

# 3. Authentication Administrator

Solo administra la autenticación.

Puede:

- Restablecer MFA.
- Eliminar dispositivos Microsoft Authenticator.
- Restablecer FIDO2 Passkeys.
- Restablecer Windows Hello for Business.
- Administrar métodos de autenticación.

No puede:

- Crear usuarios.
- Eliminar usuarios.
- Asignar permisos RBAC.

> Equivalente aproximado: administrador de autenticación (PAM/MFA).

---

# 4. User Access Administrator

Este rol pertenece a **Azure RBAC**, no a Microsoft Entra ID.

Su función es administrar permisos sobre recursos Azure.

Puede crear, modificar y eliminar:

```text
Role Assignment
```

Ejemplo:

```text
Ricard

↓

Contributor

↓

Subscription
```

No puede:

- Crear usuarios.
- Restablecer contraseñas.
- Configurar MFA.

> Equivalente aproximado: administrar permisos (`chmod`, `chown`, `setfacl`, `sudoers`).

---

# 5. Owner

El rol **Owner** pertenece a **Azure RBAC**.

Tiene control total sobre el ámbito donde está asignado.

Puede administrar:

- Management Groups
- Subscriptions
- Resource Groups
- Resources

Además puede:

- Crear recursos.
- Modificar recursos.
- Eliminar recursos.
- Asignar roles RBAC.

Ejemplo:

```text
Subscription

↓

Owner

↓

Puede administrar toda la suscripción
y asignar permisos a otros usuarios.
```

> Es el equivalente más cercano al usuario **root** dentro del **Management Control Plane**.

---

# Ejemplos de cada plano

## 1. Identity Control Plane

**Rol utilizado:** **Global Administrator**

```text
Global Administrator

↓

Microsoft Entra ID

↓

Crear usuario

↓

Asignar licencia

↓

Configurar MFA
```

---

## 2. Management Control Plane

**Rol utilizado:** **Owner**

```text
Owner

↓

Azure Resource Manager

↓

Crear VM

↓

Crear Storage Account

↓

Asignar Contributor
```

---

## 3. Data Plane

**Rol utilizado:** **Storage Blob Data Contributor**

```text
Storage Blob Data Contributor

↓

Storage Account

↓

Blob

↓

Leer

↓

Escribir

↓

Eliminar
```

Otros ejemplos de roles del Data Plane:

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner
- Key Vault Administrator
- Key Vault Secrets User

> No existe un único "root". Cada servicio implementa sus propios roles.

---

## 4. Billing Plane

**Rol utilizado:** **Billing Account Owner**

```text
Billing Account Owner

↓

Billing Account

↓

Invoices

↓

Cost Management

↓

Payment Methods
```

Puede:

- Descargar facturas.
- Consultar costes.
- Administrar presupuestos.
- Cambiar métodos de pago.

> No existe un único "root". Depende del modelo de facturación (MCA, EA, CSP...).

---

# Global Administrator vs Owner

Aunque ambos son el "root" de su plano, **no administran lo mismo**.

| Control Plane | "Root" | ¿Qué administra? |
|---------------|:------:|------------------|
| **Identity Control Plane** | **Global Administrator** | Usuarios, grupos, aplicaciones, licencias, MFA, Conditional Access, Identity Governance, etc. |
| **Management Control Plane** | **Owner** | Recursos Azure (VMs, VNets, Storage Accounts, etc.) y asignaciones RBAC sobre su ámbito. |

---

# ¿Qué ocurre con el Root Management Group?

Un **Global Administrator** **no administra automáticamente todos los recursos Azure**.

Puede utilizar:

```text
Elevate access
```

Esta acción le asigna automáticamente el rol:

```text
User Access Administrator
```

sobre el:

```text
Root Management Group (/)
```

Desde ese momento podrá asignarse el rol **Owner** sobre cualquier ámbito de Azure.

---

# Flujo completo

```text
Global Administrator

        │
        ▼
Elevate access

        │
        ▼
User Access Administrator
sobre Root Management Group

        │
        ▼
Se asigna

Owner

        │
        ▼

Management Group
Subscription
Resource Group
Resource
```

---

# ¿Por qué Microsoft lo hace así?

Por seguridad.

Microsoft separa claramente:

- **Identity Control Plane** → Identidades.
- **Management Control Plane** → Recursos.

Así un administrador de identidades no obtiene automáticamente control sobre todas las VMs, Storage Accounts o redes del tenant.

---

# Regla mnemotécnica

| Rol | Piensa en... |
|------|--------------|
| **Global Administrator** | El **root** del **Identity Control Plane**. |
| **User Administrator** | Administra usuarios. |
| **Authentication Administrator** | Administra la autenticación. |
| **User Access Administrator** | Administra quién tiene permisos sobre los recursos Azure. |
| **Owner** | El **root** del **Management Control Plane** (sobre su ámbito). |

---

# Ejemplos típicos del AZ-104

| Necesidad | Rol adecuado |
|-----------|--------------|
| Crear un usuario | **User Administrator** |
| Restablecer una contraseña | **User Administrator** |
| Restablecer MFA | **Authentication Administrator** |
| Asignar Contributor a una VM | **User Access Administrator** o **Owner** |
| Administrar todos los recursos de una suscripción | **Owner** |
| Administrar Microsoft Entra ID | **Global Administrator** |

---

> [!IMPORTANT]
> ## Claves para el AZ-104
>
> Existen dos "root" claramente definidos:
>
> - **Global Administrator** → Root del **Identity Control Plane**.
> - **Owner** → Root del **Management Control Plane** (sobre el ámbito asignado).
>
> Además:
>
> - **User Administrator** administra usuarios.
> - **Authentication Administrator** administra la autenticación.
> - **User Access Administrator** administra los **Role Assignments** de Azure RBAC.
>
> Un **Global Administrator** **no administra automáticamente todos los recursos Azure**. Si necesita hacerlo, debe utilizar **Elevate access**, que le asigna el rol **User Access Administrator** sobre el **Root Management Group**, desde donde podrá asignarse el rol **Owner**.

---

# Comparativa de roles

| Rol | Pertenece a | ¿Es el "root" de su Control Plane? | Equivalente aproximado en Linux | ¿Qué administra? | Ejemplo |
|------|-------------|:----------------------------------:|---------------------------------|------------------|----------|
| **Global Administrator** | **Microsoft Entra ID** | ✅ Sí | **root** | Administración total del tenant de Microsoft Entra ID (usuarios, grupos, aplicaciones, licencias, Conditional Access, Identity Governance, etc.). | Crear usuarios, asignar licencias, configurar MFA, crear aplicaciones Enterprise, etc. |
| **User Administrator** | **Microsoft Entra ID** | ❌ No | Administrador de usuarios (`useradd`, `passwd`) | Gestiona usuarios y grupos. Puede crear, modificar y eliminar usuarios, restablecer contraseñas y asignar determinadas licencias. | Crear un usuario y restablecer su contraseña. |
| **Authentication Administrator** | **Microsoft Entra ID** | ❌ No | Administrador de autenticación/PAM | Gestiona los métodos de autenticación (MFA, Microsoft Authenticator, FIDO2, Windows Hello for Business, etc.). | Restablecer el registro MFA de un usuario. |
| **User Access Administrator** | **Azure RBAC** | ❌ No | Administrador de permisos (`chmod`, `chown`, `setfacl`, `sudoers`) | Administra los **Role Assignments** de Azure RBAC. Decide quién tiene acceso a los recursos Azure, pero no administra los propios recursos. | Asignar el rol **Contributor** sobre una suscripción a un usuario. |
| **Owner (Azure RBAC)** | **Azure RBAC** | ✅ Sí (sobre su ámbito) | **root** sobre una Subscription, Resource Group o Resource | Administración total de los recursos del ámbito asignado. Además, puede crear y eliminar **Role Assignments**. | Administrar una suscripción completa y asignar el rol **Reader** a otros usuarios. |

---

# 1. Global Administrator

Es el rol con más privilegios de **Microsoft Entra ID**.

Puede administrar prácticamente todo el tenant.

Ejemplos:

- Usuarios
- Grupos
- Aplicaciones
- Licencias
- Conditional Access
- Privileged Identity Management (PIM)
- Identity Governance
- Métodos de autenticación
- Configuración del tenant

> Es el equivalente más cercano al usuario **root** dentro del **Identity Control Plane**.

---

# 2. User Administrator

Gestiona identidades.

Puede:

- Crear usuarios.
- Eliminar usuarios.
- Modificar usuarios.
- Restablecer contraseñas.
- Administrar grupos.
- Asignar determinadas licencias.

No administra permisos RBAC sobre recursos Azure.

> Equivalente aproximado: **`useradd`**, **`passwd`**.

---

# 3. Authentication Administrator

Solo administra la autenticación.

Puede:

- Restablecer MFA.
- Eliminar dispositivos Microsoft Authenticator.
- Restablecer FIDO2 Passkeys.
- Restablecer Windows Hello for Business.
- Administrar métodos de autenticación.

No puede:

- Crear usuarios.
- Eliminar usuarios.
- Asignar permisos RBAC.

> Equivalente aproximado: administrador de autenticación (PAM/MFA).

---

# 4. User Access Administrator

Este rol pertenece a **Azure RBAC**, no a Microsoft Entra ID.

Su función es administrar permisos sobre recursos Azure.

Puede crear, modificar y eliminar:

```text
Role Assignment
```

Ejemplo:

```text
Ricard

↓

Contributor

↓

Subscription
```

No puede:

- Crear usuarios.
- Restablecer contraseñas.
- Configurar MFA.

> Equivalente aproximado: administrar permisos (`chmod`, `chown`, `setfacl`, `sudoers`).

---

# 5. Owner

El rol **Owner** pertenece a **Azure RBAC**.

Tiene control total sobre el ámbito donde está asignado.

Puede administrar:

- Management Groups
- Subscriptions
- Resource Groups
- Resources

Además puede:

- Crear recursos.
- Modificar recursos.
- Eliminar recursos.
- Asignar roles RBAC.

Ejemplo:

```text
Subscription

↓

Owner

↓

Puede administrar toda la suscripción
y asignar permisos a otros usuarios.
```

> Es el equivalente más cercano al usuario **root** dentro del **Azure RBAC Control Plane**.

---

# Global Administrator vs Owner

Aunque ambos son el "root" de su respectivo Control Plane, **no administran lo mismo**.

| Control Plane | "Root" | ¿Qué administra? |
|---------------|:------:|------------------|
| **Microsoft Entra ID** | **Global Administrator** | Usuarios, grupos, aplicaciones, licencias, MFA, Conditional Access, Identity Governance, etc. |
| **Azure RBAC** | **Owner** | Recursos Azure (VMs, VNets, Storage Accounts, etc.) y asignaciones de roles RBAC dentro de su ámbito. |

---

# ¿Qué ocurre con el Root Management Group?

Un **Global Administrator** **no administra automáticamente todos los recursos Azure**.

Si necesita hacerlo puede utilizar:

```text
Elevate access
```

Esta acción le asigna automáticamente el rol:

```text
User Access Administrator
```

sobre el:

```text
Root Management Group (/)
```

---

# Flujo completo

```text
Global Administrator

        │
        ▼
Elevate access

        │
        ▼
User Access Administrator
sobre Root Management Group

        │
        ▼
Puede asignarse

Owner

sobre cualquier:

• Management Group
• Subscription
• Resource Group
• Resource
```

---

# ¿Por qué Microsoft lo hace así?

Por seguridad.

Microsoft separa:

- La administración de **identidades**.
- La administración de **recursos**.

Así, un administrador de identidades no obtiene automáticamente permisos sobre todas las máquinas virtuales, redes o bases de datos.

---

# Regla mnemotécnica

| Rol | Piensa en... |
|------|--------------|
| **Global Administrator** | **El "root" del Identity Control Plane (Microsoft Entra ID).** |
| **User Administrator** | **Administra usuarios.** |
| **Authentication Administrator** | **Administra cómo se autentican los usuarios.** |
| **User Access Administrator** | **Administra quién tiene acceso a los recursos Azure (Azure RBAC).** |
| **Owner** | **El "root" del Azure RBAC Control Plane (sobre su ámbito).** |

---

# Ejemplos típicos del AZ-104

| Necesidad | Rol adecuado |
|-----------|--------------|
| Crear un usuario nuevo | **User Administrator** |
| Restablecer la contraseña de un usuario | **User Administrator** |
| Restablecer el registro MFA | **Authentication Administrator** |
| Asignar el rol Contributor sobre una VM | **User Access Administrator** o **Owner** |
| Asignar Owner sobre una suscripción | **User Access Administrator** o **Owner** |
| Administrar todos los recursos de una suscripción | **Owner** |
| Administrar todo Microsoft Entra ID | **Global Administrator** |

---

# Clave para el AZ-104

No confundas:

- **Global Administrator** → Administra **Microsoft Entra ID (Identity Control Plane)**.
- **User Administrator** → Administra **usuarios y grupos**.
- **Authentication Administrator** → Administra **la autenticación** de los usuarios.
- **User Access Administrator** → Administra **Azure RBAC** (Role Assignments), pero **no** los recursos.
- **Owner** → Administra **los recursos Azure** y también puede **asignar roles RBAC**.

> [!IMPORTANT]
> **La diferencia que más suele preguntar el AZ-104**
>
> Existen dos "root" distintos:
>
> - **Global Administrator** → Root del **Identity Control Plane** (Microsoft Entra ID).
> - **Owner** → Root del **Azure RBAC Control Plane** (sobre el ámbito donde está asignado).
>
> Un **Global Administrator** no administra automáticamente todos los recursos Azure. Si necesita hacerlo, debe utilizar **Elevate access**, que le asigna el rol **User Access Administrator** sobre el **Root Management Group**, desde donde podrá asignarse el rol **Owner** sobre cualquier Management Group, Subscription, Resource Group o Resource.
