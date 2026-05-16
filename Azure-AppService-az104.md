[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Azure Service Endpoint Policy - Explicación pregunta AZ-104](#azure-service-endpoint-policy---explicación-pregunta-az-104)
- [Azure App Service – Web Deploy, Microsoft Entra ID y RBAC](#azure-app-service--web-deploy-microsoft-entra-id-y-rbac)

# Azure Service Endpoint Policy - Explicación pregunta AZ-104

---

# Concepto clave

La pregunta intenta evaluar:

```text
qué controla realmente un Service Endpoint Policy
```

y especialmente:

```text
qué ocurre cuando una subnet NO tiene Service Endpoint
```

---

# Arquitectura del escenario

## VNets

| VNet | Región |
|---|---|
| VNet-West | West Europe |
| VNet-Asia | Southeast Asia |
| VNet-US | South Central US |

---

# Subnets

| Subnet | VNet | Service Endpoint |
|---|---|---|
| Subnet-A | VNet-West | ❌ None |
| Subnet-B | VNet-Asia | ✅ Microsoft.Storage |
| Subnet-C | VNet-US | ✅ Microsoft.Storage |
| Subnet-D | VNet-US | ❌ None |

---

# Storage Accounts

| Storage Account | Región |
|---|---|
| storagewest01 | West Europe |
| storageus01 | South Central US |
| storageasia01 | Southeast Asia |

---

# La policy

```text
StoragePolicy01
```

↓

Región:

```text
South Central US
```

↓

Permite acceso a:

```text
TODOS los storage accounts de la subscripción
```

---

# La frase a evaluar

```text
"Only storageus01 can be accessed from VNet-US"
```

---

# La respuesta correcta es

```text
False
```

---

# La gran trampa de la pregunta

Muchos estudiantes piensan:

```text
la Service Endpoint Policy
bloquea automáticamente todo lo demás
```

❌ Incorrecto.

---

# Lo que SÍ hace la policy

La policy solo controla tráfico que usa:

```text
Service Endpoints
```

---

# Subnet-C

## Tiene:

```text
Microsoft.Storage Service Endpoint
```

↓

y además usa:

```text
StoragePolicy01
```

---

# Entonces Subnet-C puede acceder a

```text
TODOS los storage accounts permitidos por la policy
```

↓

La policy permite:

- storagewest01
- storageus01
- storageasia01

↓

Por tanto:

```text
Subnet-C puede acceder a los tres
```

---

# Importante

La policy:

```text
NO limita por región
```

---

# Otra trampa importante

Muchos piensan:

```text
Service Endpoint Policy solo permite storage accounts de la misma región
```

❌ Incorrecto.

---

# Entonces…

## Desde Subnet-C

Puede acceder a:

| Storage Account | Acceso |
|---|---|
| storagewest01 | ✅ |
| storageus01 | ✅ |
| storageasia01 | ✅ |

porque la policy los permite todos.

---

# Pero la pregunta todavía tiene otra trampa

## Subnet-D

NO tiene:

```text
Service Endpoint
```

---

# Entonces…

Subnet-D puede seguir usando:

```text
Internet pública
```

para acceder a cualquier storage account público.

---

# MUY importante

Service Endpoint Policy:

```text
NO controla tráfico Internet normal
```

---

# Entonces desde VNet-US

Hay dos caminos:

| Subnet | Tipo acceso |
|---|---|
| Subnet-C | Service Endpoint + Policy |
| Subnet-D | Internet pública |

---

# Resultado final

Desde:

```text
VNet-US
```

se puede acceder a:

- storagewest01
- storageus01
- storageasia01

---

# Por qué la afirmación es FALSE

Porque:

```text
NO solo storageus01 es accesible
```

---

# Concepto crítico examen

Service Endpoint Policies:

✅ controlan tráfico vía Service Endpoint  
❌ NO bloquean tráfico Internet público  

---

# Arquitectura conceptual

## Subnet-C

```text
Subnet-C
   ↓
Service Endpoint
   ↓
StoragePolicy01
   ↓
Allowed Storage Accounts
```

---

## Subnet-D

```text
Subnet-D
   ↓
Internet pública
   ↓
Storage Public Endpoint
```

---

# Otra trampa importante

La región de la policy:

```text
South Central US
```

NO significa:

```text
solo Storage Accounts South Central US
```

---

# La región de la policy depende de

```text
la VNet/subnet
```

NO del Storage Account.

---

# Reglas rápidas AZ-104

```text
Service Endpoint Policies only affect traffic using Service Endpoints.
```

```text
Subnets without Service Endpoints can still access public endpoints over the Internet.
```

```text
Service Endpoint Policies do not automatically restrict Internet access.
```

---

# Frases clave AZ-104

```text
A Service Endpoint Policy controls access to Azure services through Service Endpoints only.
```

```text
Traffic that bypasses Service Endpoints is not controlled by the policy.
```

```text
Storage accounts in different regions can still be allowed by the same Service Endpoint Policy.
```

---

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
