[AZ104-INDEX](./readme.md)

# 16 - Azure App Service (AZ-104)

> Este documento resume la teoría de **Azure App Service** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure App Service?
- App Service Plan
- Web Apps
- Runtime Stack
- Deployment Slots
- Escalado
- Autoscale
- Custom Domains
- Certificados SSL
- Backup
- Web Deploy
- Authentication
- Containers
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure App Service?

**Azure App Service** es un servicio **PaaS** que permite alojar aplicaciones web, APIs y aplicaciones basadas en contenedores sin administrar máquinas virtuales.

Microsoft se encarga de:

- Sistema operativo.
- Parches.
- Escalabilidad.
- Alta disponibilidad.

---

# 2. App Service Plan

El **App Service Plan** define los recursos utilizados por las aplicaciones.

Determina:

- CPU.
- Memoria.
- Región.
- Sistema operativo.
- Escalabilidad.
- Precio.

Varias Web Apps pueden compartir el mismo App Service Plan.

---

# 3. Web Apps

Una **Web App** representa la aplicación web que se ejecuta dentro de un App Service Plan.

Puede ejecutar aplicaciones desarrolladas con:

- .NET
- Java
- Node.js
- PHP
- Python
- Contenedores Docker

---

# 4. Runtime Stack

El **Runtime Stack** define el entorno de ejecución de la aplicación.

Ejemplos:

- .NET
- Node.js
- Java
- Python
- PHP

Puede modificarse posteriormente según el lenguaje soportado.

---

# 5. Deployment Slots

Los **Deployment Slots** permiten desplegar nuevas versiones de una aplicación sin afectar al entorno de producción.

Ejemplo:

```
Production

↓

Staging

↓

Swap
```

El intercambio (**Swap**) se realiza prácticamente sin interrupción del servicio.

---

# 6. Slot Swap

El **Swap** intercambia el contenido entre dos Deployment Slots.

Permite:

- Actualizaciones con mínimo tiempo de inactividad.
- Validar una nueva versión antes de pasarla a producción.
- Rollback rápido si aparecen problemas.

---

# 7. Escalado

Azure App Service admite dos tipos de escalado.

## Escalado Vertical (Scale Up)

Aumenta los recursos de la instancia:

- CPU.
- Memoria.

No aumenta el número de instancias.

---

## Escalado Horizontal (Scale Out)

Incrementa el número de instancias de la aplicación.

Azure distribuye automáticamente el tráfico entre ellas.

---

# 8. Autoscale

El escalado automático puede basarse en:

- CPU.
- Memoria.
- Horarios.
- Métricas de Azure Monitor.

Permite aumentar o reducir automáticamente el número de instancias.

---

# 9. Custom Domains

Una Web App puede utilizar un dominio personalizado.

Ejemplo:

```
www.contoso.com
```

Para ello es necesario configurar los registros DNS correspondientes.

---

# 10. Certificados SSL

Azure App Service permite proteger aplicaciones mediante certificados SSL/TLS.

Pueden utilizarse:

- Certificados administrados por Azure.
- Certificados propios.

El tráfico HTTPS es la opción recomendada.

---

# 11. Backup

Azure App Service permite realizar copias de seguridad de:

- Aplicación.
- Configuración.
- Contenido.

Las copias se almacenan en un **Azure Storage Account**.

No utilizan un **Recovery Services Vault**.

---

# 12. Web Deploy

**Web Deploy** permite publicar aplicaciones directamente desde Visual Studio u otras herramientas.

Para utilizar autenticación mediante **Microsoft Entra ID** con el menor privilegio posible, Microsoft recomienda asignar el rol:

**Website Contributor**

Este rol permite desplegar aplicaciones sin conceder permisos administrativos completos sobre otros recursos de Azure.

---

# 13. Authentication

Azure App Service puede integrarse con distintos proveedores de autenticación.

El más habitual es:

**Microsoft Entra ID**

También soporta:

- Google
- Facebook
- GitHub
- Twitter

---

# 14. Containers

Azure App Service puede ejecutar aplicaciones basadas en contenedores.

Soporta:

- Contenedores Linux.
- Contenedores Windows.

Las imágenes pueden almacenarse en:

- Azure Container Registry.
- Docker Hub.

---

# 15. App Service vs Virtual Machine

| App Service | Virtual Machine |
|--------------|-----------------|
| PaaS | IaaS |
| Microsoft administra el SO | El cliente administra el SO |
| Escalado sencillo | Mayor administración |
| Ideal para aplicaciones web | Uso general |

---

# 16. Buenas prácticas

Microsoft recomienda:

- Utilizar **Deployment Slots** para despliegues en producción.
- Habilitar **HTTPS** mediante certificados SSL.
- Utilizar **Autoscale** para aplicaciones con carga variable.
- Almacenar las imágenes de contenedores en **Azure Container Registry**.
- Aplicar el principio de mínimo privilegio utilizando el rol **Website Contributor** para Web Deploy.

---

# Preguntas trampa del AZ-104

✅ Un **App Service Plan** puede alojar varias **Web Apps**.

✅ Los **Deployment Slots** permiten desplegar nuevas versiones sin interrumpir producción.

✅ El **Swap** intercambia el contenido entre dos Slots.

✅ El escalado **Vertical** aumenta recursos; el **Horizontal** aumenta el número de instancias.

✅ Los backups de **App Service** se almacenan en un **Storage Account**, no en un **Recovery Services Vault**.

✅ **Azure App Service** soporta **contenedores Linux y Windows**.

✅ El rol recomendado para publicar aplicaciones mediante **Web Deploy** utilizando **Microsoft Entra ID** es **Website Contributor**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure App Service | Plataforma PaaS para aplicaciones web |
| App Service Plan | CPU, memoria y escalado |
| Web App | Aplicación alojada |
| Runtime Stack | .NET, Java, Node, Python... |
| Deployment Slot | Entorno de despliegue |
| Slot Swap | Cambio sin interrupción |
| Scale Up | Más recursos |
| Scale Out | Más instancias |
| Autoscale | Escalado automático |
| Custom Domain | Dominio propio |
| SSL/TLS | HTTPS |
| Backup | Storage Account |
| Web Deploy | Publicación desde Visual Studio |
| Website Contributor | Rol recomendado para Web Deploy |
| Containers | Linux y Windows |
