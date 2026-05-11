[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure App Service – Web Deploy, Microsoft Entra ID y RBAC

## Introducción

En Azure App Service existen diferentes formas de desplegar aplicaciones web.  
Una de las más habituales es **Web Deploy (MSDeploy)**.

Cuando una organización quiere que los desarrolladores publiquen aplicaciones usando sus cuentas corporativas de Microsoft Entra ID (antes Azure Active Directory), es importante entender:

- Cómo funciona la autenticación
- Qué permisos son necesarios
- Qué roles RBAC deben asignarse
- Cómo aplicar el principio de mínimo privilegio (Least Privilege)

---

# Conceptos principales

---

# Azure App Service

## ¿Qué es?

Azure App Service es un servicio PaaS de Azure para alojar:

- Aplicaciones Web
- APIs REST
- Backends móviles
- Aplicaciones en .NET, Java, Node.js, Python, PHP, etc.

Microsoft gestiona:

- Sistema operativo
- Parches
- Escalado
- Alta disponibilidad
- Infraestructura

El cliente solo gestiona la aplicación.

---

# Web Deploy (MSDeploy)

## ¿Qué es?

Web Deploy es una tecnología de Microsoft que permite publicar aplicaciones web hacia IIS o Azure App Service.

También se conoce como:

- MSDeploy
- Web Deploy

## ¿Qué permite?

Permite desplegar:

- Código web
- Configuración
- Archivos
- Paquetes ZIP
- Cambios incrementales

## Herramientas compatibles

- Visual Studio
- Azure DevOps
- GitHub Actions
- Scripts PowerShell
- Azure CLI

---

# Microsoft Entra ID

## ¿Qué es?

Microsoft Entra ID es el servicio de identidad de Microsoft.

Anteriormente se llamaba:

- Azure Active Directory (Azure AD)

## Funciones principales

Permite:

- Autenticación de usuarios
- Single Sign-On (SSO)
- Gestión de grupos
- MFA
- Control de acceso

## Ejemplo

Un desarrollador inicia sesión en Visual Studio usando:

```text
usuario@empresa.com
```

y Azure valida su identidad mediante Microsoft Entra ID.

---

# RBAC (Role-Based Access Control)

## ¿Qué es?

RBAC es el sistema de autorización de Azure.

Define:

- Qué puede hacer un usuario
- Sobre qué recursos
- Mediante roles

---

# Diferencia entre autenticación y autorización

| Concepto | Significado |
|---|---|
| Autenticación | Quién eres |
| Autorización | Qué puedes hacer |

## Ejemplo

### Autenticación

Microsoft Entra ID valida:

```text
Ricard es un usuario válido
```

### Autorización

RBAC valida:

```text
Ricard puede desplegar en PortalApp
```

---

# Principio de mínimo privilegio (Least Privilege)

## ¿Qué significa?

Un usuario debe tener únicamente los permisos estrictamente necesarios.

Nunca más permisos de los requeridos.

---

# Ejemplo correcto

Un desarrollador necesita:

- desplegar código
- reiniciar la web

Entonces:

```text
Website Contributor
```

puede ser suficiente.

---

# Ejemplo incorrecto

Asignar:

```text
Owner
```

es excesivo porque también permite:

- asignar permisos RBAC
- borrar recursos
- modificar IAM
- controlar completamente el recurso

---

# Roles RBAC relevantes

---

# Website Contributor

## ¿Qué permite?

Permite administrar un Azure App Service.

Incluye:

- Deploy de aplicaciones
- Gestión del sitio web
- Reinicios
- Configuración básica

## ¿Qué NO permite?

No permite:

- Gestionar RBAC
- Asignar permisos
- Control total de la suscripción

## Caso de uso típico

Desarrolladores que necesitan desplegar aplicaciones.

## Por qué es el rol correcto

Porque:

- funciona con autenticación Entra ID
- sigue el principio de mínimo privilegio
- permite despliegues Web Deploy

---

# Contributor

## ¿Qué permite?

Permite administrar casi cualquier recurso Azure.

## Limitación importante

NO puede gestionar permisos RBAC.

---

# Owner

## ¿Qué permite?

Control total del recurso.

Incluye:

- Todas las operaciones
- Gestión de permisos
- Delegación RBAC

## Problema

Viola el principio de mínimo privilegio.

---

# FTPS Deployment Credentials

Azure App Service soporta despliegue mediante:

- FTP
- FTPS

usando credenciales especiales.

---

# Application-Level FTPS Credentials

## ¿Qué son?

Credenciales compartidas para toda la aplicación.

Ejemplo:

```text
usuario-app
password-app
```

## Problemas

- No usan Entra ID
- Son compartidas
- Menor trazabilidad
- Menor seguridad

---

# User-Level FTPS Credentials

## ¿Qué son?

Credenciales individuales por usuario para despliegue FTP/FTPS.

## Problemas

Aunque son por usuario:

- siguen siendo credenciales FTP
- no usan RBAC moderno
- no usan autenticación Entra ID para autorización

---

# Comparativa de opciones

| Opción | Usa Entra ID | Sigue least privilege | Recomendada |
|---|---|---|---|
| Owner | Sí | ❌ | ❌ |
| Website Contributor | Sí | ✅ | ✅ |
| App-level FTPS | ❌ | ❌ | ❌ |
| User-level FTPS | Parcial | ❌ | ❌ |

---

# Flujo correcto del escenario

## Escenario de la pregunta

La empresa quiere:

- desplegar usando Web Deploy
- autenticarse con Microsoft Entra ID
- aplicar least privilege

## Solución correcta

Asignar:

```text
Website Contributor
```

sobre el App Service.

---

# Arquitectura conceptual

```text
Developer
   │
   │ Login
   ▼
Microsoft Entra ID
   │
   │ Token OAuth
   ▼
Azure RBAC
   │
   │ Verifica rol Website Contributor
   ▼
Azure App Service
   │
   ▼
Web Deploy permitido
```

---

# Puntos importantes para el examen AZ-104

## Debes recordar

### Web Deploy + Entra ID

→ usar RBAC

### Least Privilege

→ evitar Owner

### Rol típico para App Service deployment

→ Website Contributor

### FTPS credentials

→ NO usan el modelo moderno recomendado de Entra ID + RBAC

---

# Resumen rápido

| Concepto | Idea clave |
|---|---|
| App Service | Hosting PaaS para aplicaciones web |
| Web Deploy | Tecnología de despliegue |
| Entra ID | Autenticación |
| RBAC | Autorización |
| Website Contributor | Rol correcto para deploy |
| Owner | Demasiados permisos |
| FTPS credentials | Modelo antiguo basado en usuario/password |

---

# Referencias oficiales

## Azure RBAC Built-in Roles

https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#website-contributor

## Azure App Service Deployment

https://learn.microsoft.com/en-us/azure/app-service/deploy-best-practices

## Microsoft Entra ID

https://learn.microsoft.com/en-us/entra/fundamentals/whatis
