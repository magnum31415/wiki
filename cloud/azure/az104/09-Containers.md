[AZ104-INDEX](./readme.md)

# 09 - Containers (AZ-104)

> Este documento resume la teoría de **Azure Containers** más preguntada en el examen **AZ-104**.

---

# Índice

- Contenedores
- Azure Container Instances (ACI)
- Container Groups
- Azure Container Apps (ACA)
- Azure Kubernetes Service (AKS)
- App Service
- Contenedores Windows y Linux
- Casos de uso
- Comparativa de servicios
- Preguntas trampa

---

# 1. ¿Qué es un contenedor?

Un **contenedor** es una unidad ligera que incluye:

- Aplicación.
- Librerías.
- Dependencias.
- Configuración.

Todos los contenedores comparten el kernel del sistema operativo anfitrión, por lo que consumen menos recursos que una máquina virtual.

---

# 2. Contenedores vs Máquinas Virtuales

| Contenedor | Máquina Virtual |
|------------|-----------------|
| Comparte el kernel | Sistema operativo completo |
| Inicio en segundos | Inicio más lento |
| Ligero | Más pesado |
| Muy portable | Menos portable |

---

# 3. Azure Container Instances (ACI)

**Azure Container Instances (ACI)** permite ejecutar contenedores sin administrar servidores ni clústeres.

Es la solución más sencilla para ejecutar uno o varios contenedores.

Ideal para:

- Procesos puntuales.
- Automatizaciones.
- Tareas programadas.
- Desarrollo y pruebas.

---

# 4. Container Group

Un **Container Group** es la unidad de despliegue de **Azure Container Instances**.

Todos los contenedores del grupo:

- Comparten la misma dirección IP.
- Comparten almacenamiento.
- Comparten ciclo de vida.
- Se ejecutan en el mismo host.

Es equivalente a un **Pod** en Kubernetes.

---

# 5. Azure Container Apps (ACA)

**Azure Container Apps** permite ejecutar aplicaciones basadas en contenedores sin administrar Kubernetes.

Proporciona automáticamente:

- Escalado automático.
- Balanceo.
- Revisiones.
- Gestión del tráfico.

Está pensado para aplicaciones modernas y microservicios.

---

# 6. Azure Kubernetes Service (AKS)

**Azure Kubernetes Service (AKS)** es un servicio administrado para ejecutar clústeres de Kubernetes.

Está orientado a:

- Grandes aplicaciones.
- Microservicios.
- Alta disponibilidad.
- Orquestación compleja.

Es la solución más potente, pero también la más compleja de administrar.

---

# 7. Azure App Service

**Azure App Service** permite ejecutar aplicaciones web y APIs.

También admite el despliegue de aplicaciones basadas en contenedores.

Puede utilizar imágenes almacenadas en:

- Azure Container Registry.
- Docker Hub.

---

# 8. Contenedores Linux

Los **contenedores Linux** pueden ejecutarse en:

- Azure Container Instances.
- Azure Container Apps.
- Azure Kubernetes Service.
- Azure App Service.

Es el tipo de contenedor más habitual en Azure.

---

# 9. Contenedores Windows

Los **contenedores Windows** solo pueden ejecutarse en:

- Azure Container Instances.
- Azure App Service.
- Azure Kubernetes Service.

**Azure Container Apps no admite contenedores Windows.**

Es una de las preguntas más repetidas del AZ-104.

---

# 10. Escalado

## Azure Container Instances

No dispone de escalado automático integrado.

---

## Azure Container Apps

Escala automáticamente según:

- HTTP.
- CPU.
- Memoria.
- Eventos.

---

## AKS

El escalado se realiza mediante Kubernetes.

Puede utilizar:

- Horizontal Pod Autoscaler.
- Cluster Autoscaler.

---

# 11. Casos de uso

## Azure Container Instances

Utilizar cuando:

- Solo se necesitan uno o pocos contenedores.
- No se requiere orquestación.

---

## Azure Container Apps

Utilizar cuando:

- Se desarrollan microservicios.
- Se necesita escalado automático.
- No se quiere administrar Kubernetes.

---

## Azure Kubernetes Service

Utilizar cuando:

- Existen decenas o cientos de contenedores.
- Se necesita orquestación avanzada.
- Se requiere alta disponibilidad.

---

# 12. Comparativa

| Servicio | Orquestación | Escalado automático | Administración |
|----------|--------------|---------------------|----------------|
| ACI | No | No | Muy baja |
| ACA | Sí | Sí | Baja |
| AKS | Completa | Sí | Alta |

---

# 13. Integración con ACR

Todos los servicios pueden utilizar imágenes almacenadas en:

**Azure Container Registry (ACR)**

Es la opción recomendada frente a registros públicos.

---

# 14. Buenas prácticas

Microsoft recomienda:

- Utilizar **ACI** para cargas simples o temporales.
- Utilizar **ACA** para microservicios sin administrar Kubernetes.
- Utilizar **AKS** cuando se requiera orquestación completa.
- Almacenar las imágenes en **Azure Container Registry**.

---

# Preguntas trampa del AZ-104

✅ Un **Container Group** es la unidad de despliegue de **Azure Container Instances**.

✅ Todos los contenedores de un **Container Group** comparten la misma **IP** y el mismo ciclo de vida.

✅ **Azure Container Apps** **solo admite contenedores Linux**.

✅ Los **contenedores Windows** pueden ejecutarse en **ACI**, **AKS** y **App Service**, pero **no** en **Azure Container Apps**.

✅ **Azure Container Instances** no proporciona orquestación.

✅ **AKS** es la solución adecuada para aplicaciones complejas basadas en Kubernetes.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Contenedor | Aplicación + dependencias |
| ACI | Ejecuta contenedores sin servidores |
| Container Group | Unidad de despliegue de ACI |
| ACA | Microservicios con escalado automático |
| AKS | Kubernetes administrado |
| App Service | También ejecuta contenedores |
| Linux Containers | ACI, ACA, AKS y App Service |
| Windows Containers | ACI, AKS y App Service |
| ACR | Registro recomendado para imágenes |
