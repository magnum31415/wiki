[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

| Concepto       | Qué es                                       |
| -------------- | -------------------------------------------- |
| Tenant         | Identidad y seguridad                        |
| Subscription   | Facturación y contenedor de recursos         |
| Resource Group | Agrupación lógica dentro de una subscription |


## Microsoft Entra tenant
Un Microsoft Entra tenant es el contenedor lógico de identidad y seguridad que representa a una organización dentro de Microsoft Cloud. Es, en la práctica, tu directorio corporativo en la nube.


**Microsoft Entra Tenant = Directorio de identidad y seguridad que contiene usuarios, apps y suscripciones de una organización en Azure.**


### 🧠 Qué significa realmente

Cuando una empresa crea su entorno en Azure o Microsoft 365, automáticamente se crea un:

Tenant de Microsoft Entra ID (antes Azure Active Directory)

Ese tenant contiene:
- 👤 Usuarios
- 👥 Grupos
- 🏢 Aplicaciones registradas
- 🔐 Roles (RBAC, Global Admin, etc.)
- 🔑 Service Principals
- 🪪 Managed Identities
- 🔄 Configuración de autenticación (MFA, Conditional Access)

### 🏗 Cómo encaja en la arquitectura

Relación jerárquica simplificada:
````
Microsoft Entra Tenant
        │
        ├── Usuarios y Grupos
        ├── Aplicaciones (App registrations)
        ├── Service Principals
        │
        └── Azure Subscriptions
                ├── Resource Groups
                └── Recursos (VMs, SQL, Storage, etc.)
````

- 👉 Una suscripción Azure pertenece a un único tenant
- 👉 Un tenant puede tener múltiples suscripciones

###🎯 En el examen AZ-305 debes recordar

- El tenant es el límite de identidad y seguridad
- RBAC se aplica dentro del tenant
- Puedes tener:
  - Single-tenant apps
  - Multi-tenant apps
- Cross-tenant access requiere configuración explícita (B2B, etc.)

## Azure Subscription

Una Azure Subscription (Suscripción de Azure) es el contenedor de facturación y aislamiento de recursos dentro de Azure.

Es el nivel donde:

- 💳 Se genera la factura
- 🔐 Se aplican permisos (RBAC)
- 📦 Se organizan los recursos

###🧠 Cómo encaja en la jerarquía

````
Microsoft Entra Tenant
        │
        └── Azure Subscriptions
                │
                ├── Resource Groups
                │       ├── VMs
                │       ├── SQL
                │       ├── Storage
                │       └── etc.
                │
                └── Políticas / RBAC / Budgets
````

- 👉 Un tenant puede tener varias suscripciones
- 👉 Cada suscripción pertenece a un solo tenant

### 🎯 Qué define una Subscription
#### 1️⃣ Facturación

- Cada suscripción tiene su método de pago
- Puedes separar costes por:
  - Producción
  - Desarrollo
  - Departamentos

#### 2️⃣ Límite administrativo

RBAC se asigna a nivel:
- Subscription
- Resource Group
- Recurso

#### 3️⃣ Límites y cuotas

Límites de:
- vCPUs
- Recursos por región
- Servicios


## 🔐 ¿Qué son los roles en Azure?

Un rol en Azure define qué acciones puede hacer un usuario, grupo o identidad administrada sobre recursos.
Forma parte de RBAC (Role-Based Access Control) y siempre se aplica así:

**Un rol en Azure es un conjunto de permisos que se asigna a una identidad sobre un alcance específico.**

````
Security Principal (usuario/grupo/app)
+ Role
+ Scope (Management Group / Subscription / RG / Recurso)
````

- 👉 Un rol no es una persona, es un conjunto de permisos.

### 🧱 Tipos de roles en Azure

Azure tiene tres grandes categorías:

1. **Azure Built-in Roles** (predefinidos por Microsoft)
2. **Custom Roles** (creados por ti)
3. **Microsoft Entra roles** (antes Azure AD roles – nivel identidad)

#### 1️⃣ Azure Built-in Roles (los del examen AZ-305)

Son roles ya creados por Microsoft.

Hay más de 100, pero en el examen aparecen siempre estos:

#####  Roles básicos (core)
| Rol                           | Qué puede hacer          | Qué NO puede hacer     | Uso típico               |
| ----------------------------- | ------------------------ | ---------------------- | ------------------------ |
| **Owner**                     | Control total            | —                      | Admin de la subscription |
| **Contributor**               | Crear/modificar recursos | No puede asignar roles | DevOps                   |
| **Reader**                    | Ver recursos             | No puede modificar     | Auditoría                |
| **User Access Administrator** | Asignar roles            | No gestiona recursos   | Gestión RBAC             |

- 📌 Examen:
  - Owner = todo
  - Contributor ≠ puede dar permisos
  - Reader = solo lectura

##### Roles específicos frecuentes en AZ-305

| Rol                         | Uso                             |
| --------------------------- | ------------------------------- |
| Network Contributor         | Gestionar redes (VNet, NSG, LB) |
| Virtual Machine Contributor | Gestionar VMs                   |
| Storage Account Contributor | Gestionar Storage               |
| SQL DB Contributor          | Gestionar bases SQL PaaS        |
| Reservations Administrator  | Gestionar reservas              |
| Backup Contributor          | Gestionar backups               |

####  2️⃣ Custom Roles

Cuando los built-in no encajan.
Permiten:
- Definir permisos exactos (Microsoft.Compute/*/read)
- Aplicarlo en scopes específicos

👉 Se usan en entornos empresariales con políticas estrictas.

#### 3️⃣ Microsoft Entra Roles (nivel identidad)

Estos NO gestionan recursos Azure, gestionan el tenant:

| Rol                       | Función                  |
| ------------------------- | ------------------------ |
| Global Administrator      | Control total del tenant |
| Application Administrator | Gestiona apps            |
| Security Administrator    | Seguridad identidad      |

📌 Error típico examen:
- RBAC ≠ Entra roles

### 🏗 Scope donde se asignan

Un rol puede asignarse en:
- Management Group
- Subscription
- Resource Group
- Recurso individual

Ejemplo: Juan solo puede modificar DEV, no PROD.

````
Juan
  └── Contributor
        └── Scope: Subscription DEV
````


**⚠️ Diferencia crítica para AZ-305**

| Concepto       | Controla                    |
| -------------- | --------------------------- |
| RBAC Role      | Recursos Azure              |
| Entra Role     | Identidad y directorio      |
| Azure Policy   | Lo que está permitido crear |
| Resource Locks | Evita borrar/modificar      |


🎯 Frase corta para memorizar

Un rol en Azure es un conjunto de permisos que se asigna a una identidad sobre un alcance específico.
