[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 📚 Fundamentos de identidad, suscripciones y roles en Azure

## 📑 Índice

- [Tabla resumen conceptual](#tabla-resumen-conceptual)
- [Microsoft Entra tenant](#microsoft-entra-tenant)
- [Azure Subscription](#azure-subscription)
- [¿Qué son los roles en Azure?](#-qué-son-los-roles-en-azure)

---

## Tabla resumen conceptual

| Concepto       | Qué es                                       |
| -------------- | -------------------------------------------- |
| Tenant         | Identidad y seguridad                        |
| Subscription   | Facturación y contenedor de recursos         |
| Resource Group | Agrupación lógica dentro de una subscription |

---

### 🔝 [Volver al índice](#-índice)
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

