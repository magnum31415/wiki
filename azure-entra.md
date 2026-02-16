[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 📚 Fundamentos de identidad, suscripciones y roles en Azure

---

# 📑 Índice

1. [Tabla resumen conceptual](#-tabla-resumen-conceptual)

2. [Microsoft Entra Tenant](#microsoft-entra-tenant)

3. [Azure Subscription](#azure-subscription)

4. [¿Qué son los roles en Azure?](#qué-son-los-roles-en-azure)

5. [Tipos de roles en Azure](#tipos-de-roles-en-azure)
   - [Azure Built-in Roles](#1️⃣-azure-built-in-roles)
   - [Custom Roles](#2️⃣-custom-roles)
   - [Microsoft Entra Roles (nivel identidad)](#3️⃣-microsoft-entra-roles-nivel-identidad)

6. [Scope donde se asignan roles](#scope-donde-se-asignan-roles)

7. [Diferencia crítica para AZ-305](#diferencia-crítica-para-az-305)

8. [Microsoft Entra ID – Conceptos clave de autenticación y acceso](#-microsoft-entra-id--conceptos-clave-de-autenticación-y-acceso)
   - [Continuous Access Evaluation (CAE)](#continuous-access-evaluation-cae)
   - [Conditional Access Policies (CAP)](#conditional-access-policies-cap)
   - [OpenID Connect (OIDC)](#openid-connect-oidc)
   - [Multi-Factor Authentication (MFA)](#multi-factor-authentication-mfa)
   - [Access Reviews](#access-reviews)

9. [Azure AD Enterprise Applications](#azure-ad-enterprise-applications)
   - [SAML-based Single Sign-On (SSO)](#-saml-based-single-sign-on-sso)
   - [Conditional Access](#-conditional-access)

10. [Azure AD Application Proxy](#azure-ad-application-proxy)

11. [Resumen rápido para examen](#-resumen-rápido-para-examen)

12. [Microsoft Entra ID Governance](#-microsoft-entra-id-governance)
   - [Azure Service: Microsoft Entra ID Governance](#-azure-service-microsoft-entra-id-governance)
   - [Feature: Access Reviews](#-feature-access-reviews)

---

# 📊 Tabla resumen conceptual

| Concepto       | Qué es                                       |
|---------------|----------------------------------------------|
| Tenant        | Identidad y seguridad                        |
| Subscription  | Facturación y contenedor de recursos         |
| Resource Group| Agrupación lógica dentro de una subscription |

---

## Microsoft Entra tenant

🔝 [Volver al índice](#-índice)

Un Microsoft Entra tenant es el contenedor lógico de identidad y seguridad que representa a una organización dentro de Microsoft Cloud.

Es tu **directorio corporativo en la nube**.

**Microsoft Entra Tenant = Directorio de identidad y seguridad que contiene usuarios, apps y suscripciones de una organización en Azure.**

### Qué contiene

- Usuarios  
- Grupos  
- Aplicaciones registradas  
- Roles (RBAC, Global Admin, etc.)  
- Service Principals  
- Managed Identities  
- Configuración MFA / Conditional Access  

### Relación jerárquica

Microsoft Entra Tenant  
→ Usuarios y Grupos  
→ Aplicaciones  
→ Azure Subscriptions  
 → Resource Groups  
  → Recursos (VMs, SQL, Storage…)

### Claves examen AZ-305

- El tenant es el límite de identidad.
- RBAC vive dentro del tenant.
- Una subscription pertenece a un único tenant.
- Un tenant puede tener múltiples subscriptions.
- Cross-tenant requiere B2B explícito.

---

## Azure Subscription

🔝 [Volver al índice](#-índice)

Una Azure Subscription es el contenedor de **facturación y aislamiento de recursos**.

Es donde:

- Se genera la factura  
- Se aplican permisos RBAC  
- Se organizan recursos  

### Encaje jerárquico

Microsoft Entra Tenant  
→ Azure Subscriptions  
 → Resource Groups  
  → Recursos  

### Qué define una subscription

1. Facturación independiente  
2. Límite administrativo  
3. Límites y cuotas (vCPU, servicios, etc.)

### Claves examen AZ-305

- Puedes separar PROD / DEV en distintas subscriptions.
- RBAC se asigna a nivel Subscription / RG / Recurso.
- Una subscription solo pertenece a un tenant.

---

## ¿Qué son los roles en Azure?

🔝 [Volver al índice](#-índice)

Un rol en Azure define qué acciones puede hacer una identidad sobre recursos.

Forma parte de RBAC (Role-Based Access Control).

**Fórmula mental para el examen:**

``Permiso RBAC = Security Principal + Role + Scope``

- Security Principal = usuario, grupo, app
- Role = conjunto de permisos
- Scope = Management Group / Subscription / RG / Recurso

Un rol no es una persona, es un conjunto de permisos.

---

# Tipos de roles en Azure

## 1️⃣ Azure Built-in Roles

Roles predefinidos por Microsoft.

### Roles típicos

# 🔐 Roles Azure más típicos (AZ-305)

| Rol | Puede hacer | No puede hacer | Escenario típico de uso |
|-----|------------|----------------|--------------------------|
| Owner | Control total sobre recursos y permisos | — | Administrador completo de una suscripción |
| Contributor | Crear y modificar recursos | Asignar roles | Equipo técnico que gestiona infraestructura |
| Reader | Ver recursos | Modificar o borrar | Auditoría o equipo de reporting |
| User Access Administrator | Asignar roles RBAC | Gestionar recursos | Equipo IAM que gestiona permisos |
| Network Contributor | Gestionar redes (VNet, NSG, LB) | Asignar roles | Equipo de networking |
| Virtual Machine Contributor | Crear y administrar VMs | Gestionar red completa | Equipo de sistemas |
| Storage Account Contributor | Gestionar cuentas de almacenamiento | Asignar roles | Equipo que administra storage |
| Storage Blob Data Contributor | Leer/escribir blobs | Gestionar configuración de la cuenta | App que necesita acceso a datos Blob |
| SQL DB Contributor | Gestionar bases Azure SQL | Gestionar servidor completo | DBA gestionando bases PaaS |
| SQL Server Contributor | Gestionar servidor SQL lógico | Acceso a datos internos | Administrador de servidor SQL |
| Cosmos DB Account Contributor | Gestionar cuentas Cosmos | Acceso granular a datos | Equipo que despliega NoSQL |
| Cosmos DB Built-in Data Contributor | Leer/escribir datos Cosmos | Configurar cuenta | Aplicación backend |
| Cosmos DB Built-in Data Reader | Leer datos Cosmos | Escribir datos | Reporting sobre Cosmos |
| Web Plan Contributor | Gestionar App Service Plan | Asignar roles | Administrador de hosting web |
| Website Contributor | Gestionar Web Apps | Cambiar plan base | Equipo DevOps |
| Application Insights Component Contributor | Gestionar App Insights | Control de recursos externos | Equipo de monitorización |
| Monitoring Contributor | Gestionar Azure Monitor | Acceso total recursos | Equipo observabilidad |
| Key Vault Contributor | Gestionar vault | Acceso a secretos | Administrador de Key Vault |
| Key Vault Secrets User | Leer secretos | Gestionar vault | Aplicación que consume secretos |
| Backup Contributor | Gestionar backups | Asignar roles | Equipo de backup |
| Reservations Administrator | Gestionar reservas de capacidad | Gestionar recursos | Equipo financiero optimizando costes |
| Cost Management Contributor | Gestionar presupuestos y costes | Crear recursos | Equipo FinOps |
| Security Admin (Entra) | Gestionar políticas seguridad tenant | Gestionar recursos Azure | Equipo seguridad identidad |
| Global Administrator (Entra) | Control total del tenant | — | Administración del directorio |
| Privileged Role Administrator | Gestionar roles Entra | Gestionar recursos Azure | Gestión de roles privilegiados |

---

## 🧠 Claves rápidas AZ-305

- Owner = todo.
- Contributor ≠ puede asignar roles.
- Data roles (Blob/Cosmos) = acceso a datos, no a infraestructura.
- Roles Entra ≠ RBAC Azure.
- FinOps → Reservations / Cost Management.
- Seguridad identidad → Entra roles.


Claves examen:

- Owner = todo.
- Contributor NO puede asignar roles.
- Reader = solo lectura.

### Roles específicos frecuentes AZ-305

- Network Contributor
- Virtual Machine Contributor
- Storage Account Contributor
- SQL DB Contributor
- Reservations Administrator
- Backup Contributor

---

## 2️⃣ Custom Roles

Cuando los built-in no encajan.

Permiten:

- Definir permisos exactos.
- Aplicar en scopes concretos.

Se usan en entornos empresariales estrictos.

---

## 3️⃣ Microsoft Entra Roles (nivel identidad)

Gestionan el tenant, no los recursos Azure.

Ejemplos:

- Global Administrator
- Application Administrator
- Security Administrator

Error típico examen:

RBAC ≠ Entra Roles

---

# Scope donde se asignan roles

Un rol puede asignarse en:

- Management Group
- Subscription
- Resource Group
- Recurso individual

Ejemplo mental:

Juan  
→ Contributor  
 → Scope: Subscription DEV  

---

# Diferencia crítica para AZ-305

| Concepto       | Controla                        |
|---------------|---------------------------------|
| RBAC Role     | Recursos Azure                  |
| Entra Role    | Identidad y directorio          |
| Azure Policy  | Lo que está permitido crear     |
| Resource Lock | Evita borrar o modificar        |

---

# Frase para memorizar

Un rol en Azure es un conjunto de permisos que se asigna a una identidad sobre un alcance específico.

---

# 🔐 Microsoft Entra ID – Conceptos clave de autenticación y acceso

---

## Continuous Access Evaluation (CAE)

🔝 [Volver al índice](#-índice)

**¿Qué es?**  
Mecanismo que permite que los tokens de acceso se validen en tiempo real, sin esperar a que expiren.

**Qué hace en la práctica**
- Revoca acceso inmediatamente si:
  - Se cambia la contraseña
  - Se deshabilita el usuario
  - Se detecta riesgo

**Clave examen AZ-305**  
CAE = Revocación casi inmediata de acceso.

---

## Conditional Access Policies (CAP)

🔝 [Volver al índice](#-índice)

Motor de políticas dinámicas que decide si un usuario puede acceder a un recurso.

Puede exigir:
- MFA
- Dispositivo compliant
- Bloqueo de acceso

**Clave examen**  
Si ocurre X → exige Y.

---

## OpenID Connect (OIDC)

🔝 [Volver al índice](#-índice)

Protocolo moderno de autenticación basado en OAuth 2.0.

Se usa para:
- Login con Microsoft
- SSO
- Apps cloud

**Clave examen**  
OIDC = Autenticación moderna.

---

## Multi-Factor Authentication (MFA)

🔝 [Volver al índice](#-índice)

Requiere más de un factor de autenticación.

**Clave examen**  
MFA reduce riesgo de credenciales comprometidas.

---

## Access Reviews

🔝 [Volver al índice](#-índice)

**¿Qué es?**  
Funcionalidad de Microsoft Entra ID (Identity Governance) que permite revisar periódicamente quién tiene acceso a qué recursos.

**Qué hace en la práctica**
- Revisa membresías de grupos
- Revisa asignaciones de roles
- Permite aprobar o revocar accesos
- Automatiza expiración de permisos

**Escenarios típicos**
- Revisar accesos de usuarios externos (B2B)
- Revisar miembros de grupos privilegiados
- Cumplimiento normativo (SOX, ISO, etc.)

**Clave examen AZ-305**
Access Reviews = Control periódico de privilegios para evitar acumulación de permisos.

---

## Azure AD Enterprise Applications

🔝 [Volver al índice](#-índice)

**Clave AZ-305**

Enterprise Applications = Gestión de acceso y autenticación de aplicaciones dentro del tenant (SSO + permisos + Conditional Access).

**¿Qué es?**  
Representa las aplicaciones que usan tu tenant para autenticarse (SaaS o apps internas).

Cuando registras o integras una app:
- Se crea un **Service Principal** en Enterprise Applications.
- Desde aquí gestionas:
  - Permisos
  - Asignación de usuarios/grupos
  - SSO
  - Conditional Access
  - Consentimientos

### 🔐 SAML-based Single Sign-On (SSO)

Las **Enterprise Applications en Microsoft Entra** permiten integrar aplicaciones (incluidas on-premises) mediante  **SAML-based Single Sign-On (SSO)**.

- Al configurar una aplicación como Enterprise Application:
  - Se establece federación basada en SAML.
  - Los usuarios se autentican en Entra ID.
  - Entra emite una SAML Assertion firmada.
  - El usuario accede a la aplicación sin volver a introducir credenciales.

👉 Resultado: **Inicio de sesión único (SSO)**.

![Azure-AD-Enterprise-App-SAML-SSO](./img/azure/Azure-AD-Enterprise-App-SAML-SSO.png)

### 🛡 Conditional Access

Las políticas de Conditional Access permiten aplicar controles de seguridad según condiciones como:
- Ubicación
- Dispositivo
- Nivel de riesgo
- Estado del usuario
Ejemplo típico:
- Si el usuario accede desde una ubicación diferente → Requerir MFA

Esto añade una capa adicional de seguridad verificando múltiples factores antes de conceder acceso.


**Diferencia clave examen**
- App Registration = definición global de la app.
- Enterprise Application = instancia en tu tenant.



---

## Azure AD Application Proxy

🔝 [Volver al índice](#-índice)

**¿Qué es?**  
Servicio que permite publicar aplicaciones on-premises de forma segura en Internet usando Microsoft Entra ID.

**Cómo funciona**
- Instalas un conector en tu red interna.
- El tráfico saliente se establece hacia Azure.
- Los usuarios acceden vía Entra ID (SSO + Conditional Access).

**Ventajas**
- No necesitas abrir puertos inbound.
- Integración con MFA y Conditional Access.
- Ideal para apps legacy web internas.

**Clave examen AZ-305**
Application Proxy = Publicar aplicaciones on-prem de forma segura usando identidad Entra.

---

# 🧠 Resumen rápido para examen

| Concepto | Qué controla |
|----------|-------------|
| CAE | Revocación inmediata de acceso |
| Conditional Access | Reglas dinámicas de acceso |
| OIDC | Protocolo de autenticación moderno |
| MFA | Verificación multifactor |
| Access Reviews | Revisión periódica de accesos |
| Enterprise Applications | Gestión de acceso a apps en el tenant |
| Application Proxy | Publicar apps on-prem con identidad Entra |


# # 🔐 Microsoft Entra ID Governance

## 🔹 Azure Service: Microsoft Entra ID Governance

Servicio cloud que permite **gestionar y controlar el acceso de usuarios a recursos** dentro de Microsoft Entra ID (antes Azure AD).

Su objetivo es asegurar que:
- Solo las personas adecuadas tengan acceso.
- El acceso sea revisado periódicamente.
- Se cumplan requisitos de seguridad y compliance.

Incluye funcionalidades como:
- Gestión del ciclo de vida de identidades.
- Control de acceso privilegiado.
- Revisiones de acceso.
- Políticas de gobernanza.

👉 En resumen: ayuda a mantener el acceso limpio, controlado y auditado.

---

## 🔹 Feature: Access Reviews

Funcionalidad dentro de Entra ID Governance que permite **revisar periódicamente quién tiene acceso a qué recursos**.

Permite:
- Crear revisiones automáticas (mensuales, trimestrales, etc.).
- Asignar revisores (ej: administradores o responsables).
- Confirmar o eliminar accesos.
- Revocar automáticamente accesos innecesarios.

Ejemplo típico:
- Revisar mensualmente qué usuarios (incluidos invitados/guest) tienen acceso a una aplicación.
- El administrador valida si aún lo necesitan.
- Si no → el acceso se elimina automáticamente.

👉 En resumen: automatiza la revisión y limpieza de permisos.

