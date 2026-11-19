[Azure](azure.md)

# 📑 Índice

- [Comparativa Completa Seguridad y Control en Azure](#-comparativa-completa-seguridad-y-control-en-azure)

## 🔐 Modelos de Control

- [Azure RBAC IAM](#1️⃣-azure-rbac-iam)
- [Microsoft Entra ID Roles](#2️⃣-microsoft-entra-id-roles-antes-azure-ad-roles)
- [Data Plane RBAC](#3️⃣-data-plane-rbac)
- [Azure Policy](#4️⃣-azure-policy)
- [Resource Locks](#5️⃣-resource-locks)
- [Azure Access Policies](#6️⃣-azure-access-policies)

## 📊 Comparativas y Ejemplos

- [Comparativa Visual](#-comparativa-visual)
- [Ejemplo completo práctico](#-ejemplo-completo-práctico)
- [Regla mental examen](#-regla-mental-examen)
- [La clave para no confundirte](#-la-clave-para-no-confundirte)

## 🧠 Arquitectura

- [Control plane vs Data plane](#control-plane-vs-data-plane)

## 📦 Data Plane práctico

- [Cómo dar permisos de lectura en blobs](#-cómo-dar-permisos-de-lectura-en-blobs-data-plane-en-azure)
- [Paso 1 Entender el error típico](#-paso-1-entender-el-error-típico)
- [Paso 2 Asignar rol correcto](#-paso-2-asignar-rol-correcto-data-plane-rbac)
- [Dónde se asigna](#-dónde-se-asigna)
- [Scope recomendado](#-scope-recomendado)
- [Alternativa SAS Token](#-alternativa-sas-token)
- [Resumen claro](#-resumen-claro)
- [Pregunta típica examen](#-pregunta-típica-examen)
- [Regla mental](#-regla-mental)

# 🔐 Comparativa Completa: Seguridad y Control en Azure

---

# 1️⃣ Azure RBAC (IAM)

📍 Qué es:
Sistema de autorización basado en roles para recursos de Azure.

📍 Controla:
El plano de control (Control Plane)

📍 Ejemplos:
- Crear una VM
- Borrar una VNet
- Asignar permisos

📍 Fórmula:
Security Principal + Role + Scope

📍 Portal:
Recurso → Access Control (IAM)

---

# 2️⃣ Microsoft Entra ID Roles (antes Azure AD roles)

📍 Qué es:
Roles administrativos del tenant de identidad.

📍 Controla:
Identidades, usuarios, grupos, aplicaciones.

📍 Ejemplos:
- Global Administrator
- Application Administrator
- Security Administrator

📍 Actúa sobre:
El directorio (no sobre recursos como VMs o VNets).

📍 Diferencia clave:
Entra roles ≠ permisos sobre recursos Azure.

---

# 3️⃣ Data Plane RBAC

📍 Qué es:
Permisos sobre los datos dentro de un recurso.

📍 Ejemplos:
- Leer blobs en una Storage Account
- Ejecutar queries en SQL
- Acceder a secretos en Key Vault

📍 Importante:
Puedes tener Reader (control plane)
y no poder leer los datos (data plane).

---

# 4️⃣ Azure Policy

📍 Qué es:
Sistema de gobernanza y cumplimiento.

📍 Controla:
Qué se puede o no desplegar.

📍 Ejemplos:
- Solo permitir regiones EU
- Obligar a usar tags
- Bloquear IP pública

📍 No da permisos.
📍 No asigna roles.

RBAC dice quién puede.
Policy dice si está permitido.

---

# 5️⃣ Resource Locks

📍 Qué es:
Mecanismo para bloquear cambios accidentales.

Tipos:
- CanNotDelete
- ReadOnly

📍 Ejemplo:
Evitar que alguien borre un RG crítico.

📍 Importante:
Lock puede bloquear incluso a un Owner.

---

# 6️⃣ Azure Access Policies

⚠️ Se usan principalmente en Azure Key Vault (modelo antiguo).

📍 Qué es:
Modelo de permisos específico para acceso a secretos, claves y certificados.

📍 Sustituido en la práctica por:
Azure RBAC for Key Vault (modelo recomendado).

📍 Diferencia:
Access Policy es específica del recurso.
RBAC es modelo unificado.

---

# 📊 Comparativa Visual

| Controla | Azure RBAC | Entra Roles | Data Plane RBAC | Policy | Locks | Access Policy |
|------------|-------------|-------------|----------------|--------|--------|---------------|
| Recursos Azure | ✅ | ❌ | ❌ | ✅ (reglas) | ✅ | ❌ |
| Identidades | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Datos dentro del recurso | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ (Key Vault) |
| Bloquea despliegues | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| Bloquea eliminación | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

---

# 🧠 Ejemplo completo práctico

Juan es Contributor en una Subscription.

✔ RBAC permite crear VM.

Pero:
Existe Policy que solo permite West Europe.

Si crea en East US:
❌ Bloqueado por Policy.

Además:
El RG tiene un Lock CanNotDelete.

Si intenta borrar:
❌ Bloqueado por Lock.

Y si intenta leer blobs:
Depende de Data Plane RBAC.

---

# 🎯 Regla mental examen

Permisos → RBAC  
Reglas → Policy  
Protección accidental → Locks  
Identidad → Entra ID Roles  
Acceso a datos → Data Plane RBAC  
Key Vault antiguo → Access Policies  

---

# 🔥 La clave para no confundirte

RBAC = Quién puede  
Policy = Qué está permitido  
Lock = Nadie lo borra  
Entra Roles = Admin del tenant  
Data Plane = Acceso a datos reales  

---

# Control plane vs Data plane

````mermaid
flowchart LR

    %% DATA PLANE
    subgraph DATA_PLANE
        direction TB
        D1[Usuario / Aplicación]
        D2[Servicio Azure<br>Storage / SQL / Service Bus]
        D3[Datos<br>Blobs / Tablas / Filas / Mensajes]

        D1 --> D2
        D2 --> D3
    end

    %% CONTROL PLANE
    subgraph CONTROL_PLANE
        direction TB
        C1[Usuario / Portal / CLI / ARM Template]
        C2[Azure Resource Manager ARM]
        C3[Recurso Azure<br>VM / Storage / VNet / SQL]

        C1 --> C2
        C2 --> C3
    end

    %% Forzar layout horizontal
    DATA_PLANE --- CONTROL_PLANE

````

# 📦 ¿Cómo dar permisos de lectura en blobs (Data Plane) en Azure?

Para permitir que alguien lea blobs dentro de una Storage Account debes usar:

👉 Azure RBAC con roles de Data Plane

NO basta con darle "Reader" en la Storage Account.

---

# 🧠 Paso 1: Entender el error típico

Rol:
Reader

Resultado:
✔ Puede ver la Storage Account
❌ No puede leer los blobs

Porque:
Reader = Control Plane
Leer blobs = Data Plane

---

# 🛠 Paso 2: Asignar rol correcto (Data Plane RBAC)

Debes asignar uno de estos roles:

- Storage Blob Data Reader → Solo lectura
- Storage Blob Data Contributor → Leer y escribir
- Storage Blob Data Owner → Control total sobre blobs

---

# 📍 Dónde se asigna

Portal:

Storage Account  
→ Access Control (IAM)  
→ Add role assignment  
→ Seleccionar:

Storage Blob Data Reader

→ Asignar al usuario / grupo / Managed Identity

---

# 📌 Scope recomendado

Puedes asignarlo en:

- Nivel Storage Account
- Nivel Resource Group
- Nivel Subscription
- Nivel Container específico (más granular)

Para mínimo privilegio:
Asignarlo a nivel de Container.

---

# 🔐 Alternativa: SAS Token

También puedes dar acceso mediante:

Shared Access Signature (SAS)

Pero:
- Es temporal
- Es menos gobernable
- No usa identidad Azure AD

En entornos empresariales se recomienda:

👉 Azure AD + RBAC

---

# 📊 Resumen claro

| Necesidad | Qué hacer |
|------------|------------|
| Ver que existe la Storage Account | Reader |
| Leer blobs | Storage Blob Data Reader |
| Subir blobs | Storage Blob Data Contributor |
| Control total blobs | Storage Blob Data Owner |

---

# 🎯 Pregunta típica examen

"Un usuario tiene Reader pero no puede descargar blobs."

Respuesta correcta:
Debe asignarse Storage Blob Data Reader.

---

# 🧠 Regla mental

Reader = Ver el recurso  
Data Reader = Leer el contenido  

---

Si quieres, te explico también cuándo necesitas habilitar "Azure AD integration" en Storage para que esto funcione correctamente, que es otra trampa frecuente.

