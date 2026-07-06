[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Azure App Service (AZ-104)](#azure-app-service-az-104)
- [Azure App Service Scaling (AZ-104)](#azure-app-service-scaling-az-104)
- [Azure App Service – Web Deploy, Microsoft Entra ID y RBAC](#azure-app-service--web-deploy-microsoft-entra-id-y-rbac)

---


# Azure App Service (AZ-104)

## Índice

- [Azure App Service (AZ-104)](#azure-app-service-az-104)
- [Qué es Azure App Service](#qué-es-azure-app-service)
- [Qué permite hacer](#qué-permite-hacer)
- [Qué tipo de servicio es](#qué-tipo-de-servicio-es)
- [Concepto clave](#concepto-clave)
- [Arquitectura básica](#arquitectura-básica)
- [Qué es un App Service Plan](#qué-es-un-app-service-plan)
- [Relación entre App Service y App Service Plan](#relación-entre-app-service-y-app-service-plan)
- [Tipos principales de App Service](#tipos-principales-de-app-service)
- [Características importantes](#características-importantes)
- [Deployment Slots](#deployment-slots)
- [Scale Up vs Scale Out](#scale-up-vs-scale-out)
- [Autoscaling](#autoscaling)
- [Autenticación y seguridad](#autenticación-y-seguridad)
- [Networking](#networking)
- [Integración con Azure](#integración-con-azure)
- [Pricing Tiers importantes](#pricing-tiers-importantes)
- [Qué suele preguntar Microsoft en AZ-104](#qué-suele-preguntar-microsoft-en-az-104)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué es Azure App Service

Azure App Service es un servicio:

```text
PaaS (Platform as a Service)
```

que permite desplegar aplicaciones web sin administrar servidores.

---

# Qué permite hacer

Permite ejecutar:

- Web Apps
- APIs
- Backends
- Aplicaciones web empresariales
- Aplicaciones .NET, Java, Node.js, Python, PHP, etc.

---

# Qué tipo de servicio es

| Tipo servicio | Azure App Service |
|---|---|
| IaaS | ❌ |
| PaaS | ✅ |
| Serverless | Parcialmente |

---

# Concepto clave

Microsoft administra:

- sistema operativo
- parches
- infraestructura
- runtime
- disponibilidad base

Tú administras:

- aplicación
- configuración
- código
- escalado

---

# Arquitectura básica

```text
Azure App Service
        ↓
App Service Plan
        ↓
Azure Infrastructure
```

---

# Qué es un App Service Plan

El **Pricing Tier** (nivel de precio) de un **Azure App Service Plan** define principalmente:

* Potencia de las instancias: CPU y RAM.
* Número máximo de instancias.
* Capacidad de escalado.
* Funcionalidades disponibles.
* Alta disponibilidad.
* Coste.

```text
App Service Plan
│
├── Operating System → Linux / Windows
│
├── Region → West Europe, Germany West Central...
│
└── Pricing Tier
    ├── Free
    ├── Shared
    ├── Basic
    ├── Standard
    ├── Premium
    └── Isolated
```

> **Cuanto mayor es el Tier, más recursos, funcionalidades, capacidad de escalado y coste.**

---

# Tier vs SKU

Dentro de cada **Tier** existen diferentes tamaños o **SKUs**.

Ejemplo:

```text
Tier: Basic
├── B1
├── B2
└── B3

Tier: Standard
├── S1
├── S2
└── S3

Tier: Premium v3
├── P0v3
├── P1v3
├── P2v3
└── P3v3
```

Por tanto:

> **Premium v3 = Tier**
> **P1v3 = SKU o tamaño concreto dentro del Tier**

---

# Principales Pricing Tiers

| Tier        | SKUs típicas | Uso principal           | Autoscale | Zone Redundancy      |
| ----------- | ------------ | ----------------------- | --------- | -------------------- |
| Free        | F1           | Pruebas                 | ❌         | ❌                    |
| Shared      | D1           | Desarrollo              | ❌         | ❌                    |
| Basic       | B1-B3        | Apps pequeñas           | ❌         | ❌                    |
| Standard    | S1-S3        | Producción              | ✅         | ❌                    |
| Premium v2  | P1v2-P3v2    | Alto rendimiento        | ✅         | ✅                    |
| Premium v3  | P0v3-P3v3    | Producción moderna      | ✅         | ✅                    |
| Isolated v2 | I1v2-I6v2    | App Service Environment | ✅         | Arquitectura aislada |

---

# Jerarquía de Tiers

```text
Free / Shared
      ↓
    Basic
      ↓
   Standard
      ↓
 Premium v2 / v3
      ↓
 Isolated v2
```

Cuanto más alto es el Tier:

```text
↑ CPU y RAM
↑ Número de instancias
↑ Capacidad de escalado
↑ Funcionalidades
↑ Alta disponibilidad
↑ Coste
```

---

# Ejemplo: interpretación de P1v3

```text
P1v3
││ └── v3 = Generación
│└──── 1 = Tamaño
└───── P = Premium
```

| SKU  | Tier       | Tamaño  |
| ---- | ---------- | ------- |
| B1   | Basic      | Pequeño |
| B3   | Basic      | Grande  |
| S1   | Standard   | Pequeño |
| P1v3 | Premium v3 | Pequeño |
| P3v3 | Premium v3 | Grande  |

---

# Variantes Memory Optimized

Algunas SKUs Premium tienen variantes optimizadas para memoria, identificadas mediante la letra `m`.

```text
P1v3  → Premium v3 estándar
P1mv3 → Premium v3 Memory Optimized
```

---

# Zone Redundancy

Para el examen AZ-104:

```text
Basic          ❌
Standard       ❌
Premium v2     ✅
Premium v3     ✅
```

Ejemplo:

```text
ASP1 → Linux   + Basic B1       ❌
ASP2 → Windows + Standard S1    ❌
ASP3 → Linux   + Premium v3     ✅
ASP4 → Windows + Premium v3     ✅
```

> **El sistema operativo Linux o Windows no determina el soporte de Zone Redundancy. El factor principal es el Pricing Tier.**

---

# Regla clave AZ-104

```text
App Service Plan
        │
        ├── Tier → Nivel de capacidades
        │
        └── SKU  → Tamaño concreto dentro del Tier
```

> **Tier = funcionalidades y capacidades disponibles.**
> **SKU = tamaño y recursos concretos.**



---

# Relación entre App Service y App Service Plan

Varias apps pueden compartir:

```text
el mismo App Service Plan
```

---

## Ejemplo

```text
App Service Plan
│
├── WebApp1
├── WebApp2
└── APIApp1
```

---

# Tipos principales de App Service

| Tipo | Uso |
|---|---|
| Web App | Sitios web |
| API App | APIs REST |
| Function App | Funciones serverless |
| Web App for Containers | Contenedores Docker |

---

# Características importantes

| Feature | Soportado |
|---|---|
| Autoscaling | ✅ |
| Deployment slots | ✅ |
| HTTPS | ✅ |
| Managed Identity | ✅ |
| Authentication integrada | ✅ |
| Custom domains | ✅ |
| SSL certificates | ✅ |

---

# Deployment Slots

Permiten tener:

- producción
- staging
- testing

---

## Ejemplo

```text
Production Slot
Staging Slot
```

---

## Uso típico

```text
Swap staging → production
```

sin downtime.

---

# Scale Up vs Scale Out

| Tipo | Qué hace |
|---|---|
| Scale Up | Más CPU/RAM |
| Scale Out | Más instancias |

---

# Scale Up

Ejemplo:

```text
S1 → P1
```

---

# Scale Out

Ejemplo:

```text
2 → 5 instancias
```

---

# Autoscaling

App Service puede escalar automáticamente usando:

- CPU
- memoria
- requests
- schedules

---

## Ejemplo típico examen

```text
CPU > 80%
```

↓

```text
añadir instancia
```

---

# Autenticación y seguridad

App Service soporta:

| Feature | Soportado |
|---|---|
| Microsoft Entra ID | ✅ |
| Managed Identity | ✅ |
| OAuth/OpenID | ✅ |
| HTTPS only | ✅ |

---

# Networking

App Service soporta:

| Feature | Soportado |
|---|---|
| VNet Integration | ✅ |
| Private Endpoint | ✅ |
| Access Restrictions | ✅ |

---

# Integración con Azure

Integraciones típicas:

| Servicio | Uso |
|---|---|
| Key Vault | Secretos |
| Storage Account | Archivos |
| Application Insights | Monitoring |
| Azure SQL | Base datos |
| Container Registry | Contenedores |

---

# Pricing Tiers importantes

| Tier | Características |
|---|---|
| Free | Testing |
| Shared | Bajo coste |
| Basic | Producción simple |
| Standard | Autoscaling |
| Premium | Alto rendimiento |
| Isolated | ASE/VNet dedicadas |

---

# Qué suele preguntar Microsoft en AZ-104

## Muy frecuente

| Tema | Importancia |
|---|---|
| Scale Up vs Scale Out | Muy alta |
| Autoscaling | Muy alta |
| App Service Plan | Muy alta |
| Deployment Slots | Alta |
| Networking | Alta |
| Managed Identity | Alta |

---

# Trampas típicas AZ-104

## Trampa 1

Confundir:

```text
App Service
```

con:

```text
App Service Plan
```

---

## Trampa 2

Pensar que:

```text
Scale Up = más instancias
```

❌ Incorrecto.

---

## Trampa 3

Pensar que cada Web App tiene su propio compute.

❌ Incorrecto.

Varias apps pueden compartir:

```text
App Service Plan
```

---

## Trampa 4

Pensar que:

```text
Deployment Slots = backup
```

❌ Incorrecto.

Son para deployment/testing.

---

# Tabla resumen examen

| Concepto | Importante |
|---|---|
| App Service | Servicio PaaS web |
| App Service Plan | Compute/scaling |
| Scale Up | Más potencia |
| Scale Out | Más instancias |
| Deployment Slot | Staging/testing |
| Autoscale | Basado métricas |

---

# Reglas rápidas AZ-104

```text
Azure App Service is a PaaS offering.
```

```text
App Service Plans define compute resources and scaling.
```

```text
Scale Up increases instance size.
```

```text
Scale Out increases the number of instances.
```

```text
Multiple apps can share the same App Service Plan.
```

---

# Frases clave AZ-104

```text
Azure App Service is used to host web applications and APIs.
```

```text
An App Service runs inside an App Service Plan.
```

```text
Autoscaling can be configured using rule-based scaling.
```

```text
Deployment slots support zero-downtime deployments.
```
---
# Azure App Service Scaling (AZ-104)

## Índice

- [Azure App Service Scaling (AZ-104)](#azure-app-service-scaling-az-104)
- [Qué es un App Service Plan](#qué-es-un-app-service-plan)
- [Scale Up vs Scale Out](#scale-up-vs-scale-out)
- [Scale Up (Vertical Scaling)](#scale-up-vertical-scaling)
- [Scale Out (Horizontal Scaling)](#scale-out-horizontal-scaling)
- [Qué son las Scale Rules](#qué-son-las-scale-rules)
- [Rule-based scaling](#rule-based-scaling)
- [Por qué la respuesta correcta es Rule-based scaling](#por-qué-la-respuesta-correcta-es-rule-based-scaling)
- [Por qué Automatic scaling es incorrecto](#por-qué-automatic-scaling-es-incorrecto)
- [Por qué Manual scaling es incorrecto](#por-qué-manual-scaling-es-incorrecto)
- [Por qué Scale Up es incorrecto en esta pregunta](#por-qué-scale-up-es-incorrecto-en-esta-pregunta)
- [Métricas típicas usadas para autoscaling](#métricas-típicas-usadas-para-autoscaling)
- [Pricing tiers importantes](#pricing-tiers-importantes)
- [Conceptos importantes examen](#conceptos-importantes-examen)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué es un App Service Plan

````mermaid
flowchart TD

    AZAPP["Azure App Service<br/>(PaaS para Web Apps, APIs, Functions)"]

    ASP["App Service Plan<br/>(CPU, RAM, Scaling, Pricing Tier)"]

    WA1["Web App 1"]
    WA2["Web App 2"]
    API1["API App 1"]
    FUNC1["Function App"]

    AZAPP --> ASP

    ASP --> WA1
    ASP --> WA2
    ASP --> API1
    ASP --> FUNC1

    SCALE["Scale Up / Scale Out"]
    SCALE --> ASP

````


Un:

```text
App Service Plan
```

define:

- compute resources
- tamaño instancias
- número instancias
- pricing tier

para:

- Web Apps
- API Apps
- Function Apps

---

# Scale Up vs Scale Out

| Tipo | Qué hace |
|---|---|
| Scale Up | Aumenta potencia instancia |
| Scale Out | Añade más instancias |

---

# Scale Up (Vertical Scaling)

Aumenta:

- CPU
- RAM
- potencia VM

---

## Ejemplo

```text
S1 → P1
```

---

## Importante

NO añade más instancias.

---

# Scale Out (Horizontal Scaling)

Añade:

```text
más instancias App Service
```

---

## Ejemplo

```text
2 instancias → 5 instancias
```

---

# Qué son las Scale Rules

Permiten escalar automáticamente basándose en:

- CPU
- memoria
- requests
- schedule
- métricas Azure Monitor

---

# Rule-based scaling

## Qué hace

Escala automáticamente cuando se cumple una condición.

---

## Ejemplo típico

```text
CPU > 80%
```

↓

```text
añadir 1 instancia
```

---

# Por qué la respuesta correcta es Rule-based scaling

La pregunta requiere:

```text
escalar automáticamente cuando CPU > 80%
```

↓

eso requiere:

✅ reglas basadas en métricas  

---

# Flujo conceptual

```text
CPU > 80%
    ↓
Scale rule triggered
    ↓
Add instances
```

---

# Por qué Automatic scaling es incorrecto

Porque en App Service:

```text
Automatic scaling
```

NO significa necesariamente:

```text
reglas basadas en métricas personalizadas
```

Microsoft busca específicamente:

```text
Rule-based scaling
```

---

# Por qué Manual scaling es incorrecto

Manual scaling:

❌ requiere intervención manual  
❌ no responde automáticamente CPU  

---

# Por qué Scale Up es incorrecto en esta pregunta

Opciones como:

```text
Premium P1
```

o:

```text
Standard S1
```

son:

```text
Scale Up
```

↓

cambian tamaño plan, pero:

❌ no crean instancias automáticas  

---

# Métricas típicas usadas para autoscaling

| Métrica | Uso típico |
|---|---|
| CPU Percentage | Muy común |
| Memory | Apps intensivas |
| HTTP Queue Length | Tráfico web |
| Requests | APIs |
| Schedule | Horarios oficina |

---

# Pricing tiers importantes

| Tier | Soporta autoscale |
|---|---|
| Free | ❌ |
| Shared | ❌ |
| Basic | ❌ limitado |
| Standard | ✅ |
| Premium | ✅ |

---

# Conceptos importantes examen

Microsoft suele evaluar:

| Concepto | Importancia |
|---|---|
| Scale Up vs Scale Out | Muy alta |
| Autoscaling | Muy alta |
| Rule-based scaling | Alta |
| CPU thresholds | Alta |
| App Service tiers | Alta |

---

# Trampas típicas AZ-104

## Trampa 1

Confundir:

```text
Scale Up
```

con:

```text
Scale Out
```

---

## Trampa 2

Pensar que cambiar tier:

```text
añade instancias automáticamente
```

❌ Incorrecto.

---

## Trampa 3

Pensar que:

```text
Manual scaling
```

sirve para autoscaling.

❌ Incorrecto.

---

## Trampa 4

Olvidar que algunos tiers:

❌ no soportan autoscale

---

# Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Más CPU/RAM | Scale Up |
| Más instancias | Scale Out |
| Escalar por CPU | Rule-based scaling |
| Escalar manualmente | Manual scaling |

---

# Reglas rápidas AZ-104

```text
Scale Up increases instance size.
```

```text
Scale Out increases the number of instances.
```

```text
Rule-based scaling uses metrics such as CPU percentage.
```

```text
Autoscaling requires supported App Service tiers.
```

---

# Frases clave AZ-104

```text
Rule-based autoscaling can scale App Service instances based on CPU usage.
```

```text
Scale out adds additional App Service instances.
```

```text
Scale up changes the pricing tier and compute size.
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
