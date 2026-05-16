[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Azure App Service Scaling (AZ-104)](#azure-app-service-scaling-az-104)
- [Azure App Service – Web Deploy, Microsoft Entra ID y RBAC](#azure-app-service--web-deploy-microsoft-entra-id-y-rbac)

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
