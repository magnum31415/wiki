
- [Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [📚 Roles en Azure](#-roles-en-azure)
- [User Administrator vs Global Administrator vs Authentication Administrator vs User Access Administrator (AZ-104)](#user-administrator-vs-global-administrator-vs-authentication-administrator-vs-user-access-administrator-az-104)

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

Estos cuatro roles pertenecen a ámbitos distintos y es muy habitual confundirlos en el AZ-104.

| Rol | Ámbito | ¿Qué puede hacer? | Ejemplo |
|------|--------|-------------------|----------|
| **Global Administrator** | Microsoft Entra ID | Administración total del tenant. Puede administrar todos los usuarios, grupos, aplicaciones, licencias y la mayoría de configuraciones del tenant. | Crear usuarios, asignar licencias, crear aplicaciones Enterprise, modificar Conditional Access, etc. |
| **User Administrator** | Microsoft Entra ID | Gestiona usuarios y grupos. Puede crear, modificar y eliminar usuarios, restablecer contraseñas y asignar determinadas licencias. | Crear un nuevo usuario y restablecer su contraseña. |
| **Authentication Administrator** | Microsoft Entra ID | Gestiona los métodos de autenticación de los usuarios. Puede restablecer MFA, FIDO2, Microsoft Authenticator y otros métodos de autenticación. | Restablecer el registro MFA de un usuario que ha perdido el móvil. |
| **User Access Administrator** | Azure RBAC | Administra el acceso a los recursos de Azure mediante Azure RBAC. Puede crear y eliminar **Role Assignments**. No administra usuarios. | Asignar el rol **Contributor** sobre una suscripción a un usuario. |

---

# 1. Global Administrator

Es el rol con más privilegios en Microsoft Entra ID.

Puede administrar prácticamente todo el tenant.

Ejemplos:

- Usuarios
- Grupos
- Aplicaciones
- Licencias
- Conditional Access
- PIM
- Identity Governance

---

# 2. User Administrator

Gestiona identidades.

Puede:

- Crear usuarios.
- Eliminar usuarios.
- Modificar usuarios.
- Restablecer contraseñas.
- Administrar grupos.

No administra permisos RBAC sobre recursos Azure.

---

# 3. Authentication Administrator

Solo administra la autenticación.

Puede:

- Restablecer MFA.
- Eliminar dispositivos Microsoft Authenticator.
- Restablecer FIDO2 Passkeys.
- Restablecer Windows Hello for Business.
- Administrar métodos de autenticación.

No puede crear usuarios.

No puede asignar permisos RBAC.

---

# 4. User Access Administrator

Este rol pertenece a **Azure RBAC**, no a Microsoft Entra ID.

Su función es administrar permisos sobre recursos Azure.

Puede crear:

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

---

# Regla mnemotécnica

| Rol | Piensa en... |
|------|--------------|
| **Global Administrator** | **Administra todo el tenant.** |
| **User Administrator** | **Administra usuarios.** |
| **Authentication Administrator** | **Administra cómo se autentican los usuarios.** |
| **User Access Administrator** | **Administra quién tiene acceso a los recursos Azure (RBAC).** |

---

# Ejemplos típicos del AZ-104

| Necesidad | Rol adecuado |
|-----------|--------------|
| Crear un usuario nuevo | **User Administrator** |
| Restablecer la contraseña de un usuario | **User Administrator** |
| Restablecer el registro MFA | **Authentication Administrator** |
| Asignar el rol Contributor sobre una VM | **User Access Administrator** |
| Asignar Owner sobre una suscripción | **User Access Administrator** |
| Administrar todo el tenant | **Global Administrator** |

---

# Clave para el AZ-104

No confundas:

- **User Administrator** → Administra **usuarios**.
- **Authentication Administrator** → Administra **la autenticación** de los usuarios.
- **User Access Administrator** → Administra **Azure RBAC** (Role Assignments).
- **Global Administrator** → Administra prácticamente **todo Microsoft Entra ID**.
