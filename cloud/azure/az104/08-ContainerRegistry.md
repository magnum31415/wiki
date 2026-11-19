[AZ104-INDEX](./readme.md)

# 08 - Azure Container Registry (ACR) (AZ-104)

> Este documento resume la teoría de **Azure Container Registry (ACR)** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure Container Registry?
- Repositories e Images
- Login Server
- Docker Login
- Docker Tag
- Docker Push
- Docker Pull
- Azure CLI
- Content Trust
- Roles RBAC
- Geo-Replication
- Private Endpoint
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Container Registry?

**Azure Container Registry (ACR)** es el servicio de Azure para almacenar imágenes de contenedores de forma privada.

Es compatible con los formatos:

- Docker
- OCI (Open Container Initiative)

Se integra con:

- Azure Kubernetes Service (AKS)
- Azure Container Apps
- Azure Container Instances (ACI)
- Azure App Service

---

# 2. Repositories

Un **Repository** agrupa distintas versiones de una misma imagen.

Ejemplo:

```
webapp

├── v1
├── v2
├── v3
└── latest
```

Cada versión se identifica mediante un **Tag**.

---

# 3. Tags

Un **Tag** identifica una versión concreta de una imagen.

Ejemplos:

```
latest

v1.0

prod

test
```

Varios tags pueden apuntar a la misma imagen.

---

# 4. Login Server

Cada ACR dispone de un **Login Server** único con el formato:

```
<nombre>.azurecr.io
```

Ejemplo:

```
contoso.azurecr.io
```

Este nombre debe utilizarse al etiquetar la imagen antes de publicarla.

---

# 5. Flujo para publicar una imagen

El proceso habitual es:

```
Crear imagen

↓

Login al ACR

↓

Docker Tag

↓

Docker Push
```

---

# 6. Login en Azure Container Registry

Antes de enviar imágenes al registro es necesario autenticarse.

Con Azure CLI:

```bash
az acr login --name MiRegistro
```

Este comando autentica Docker contra el ACR.

---

# 7. Docker Tag

Antes del **Push**, la imagen debe etiquetarse utilizando el **Login Server**.

Ejemplo:

```bash
docker tag web:v1 contoso.azurecr.io/web:v1
```

Este comando **no copia la imagen**; simplemente crea una nueva referencia.

---

# 8. Docker Push

El comando:

```bash
docker push
```

envía la imagen al Azure Container Registry.

Ejemplo:

```bash
docker push contoso.azurecr.io/web:v1
```

Solo se transfieren las capas que todavía no existen en el registro.

---

# 9. Docker Pull

Para descargar una imagen desde ACR se utiliza:

```bash
docker pull
```

Ejemplo:

```bash
docker pull contoso.azurecr.io/web:v1
```

---

# 10. Azure CLI

Los comandos más habituales son:

```bash
az acr create
```

Crear un registro.

---

```bash
az acr login
```

Autenticarse.

---

```bash
az acr repository list
```

Listar repositorios.

---

```bash
az acr repository show-tags
```

Mostrar los tags de una imagen.

---

# 11. Content Trust

**Content Trust** garantiza que únicamente puedan utilizarse imágenes firmadas.

Su objetivo es impedir el despliegue de imágenes manipuladas o no autorizadas.

Es una pregunta frecuente del examen.

---

# 12. Roles RBAC

Los roles más habituales son:

## AcrPull

Permite:

- Descargar imágenes.

No permite publicar.

---

## AcrPush

Permite:

- Descargar imágenes.
- Publicar imágenes.

Es el rol necesario para ejecutar **docker push**.

---

## Owner / Contributor

Permiten administrar el recurso Azure Container Registry.

No deben utilizarse únicamente para publicar imágenes, ya que conceden más permisos de los necesarios.

---

# 13. Geo-Replication

La **Geo-Replication** permite mantener réplicas del ACR en varias regiones.

Ventajas:

- Menor latencia.
- Mayor disponibilidad.
- Recuperación ante desastres.

Solo está disponible en el **SKU Premium**.

---

# 14. Private Endpoint

Un **Private Endpoint** permite acceder al ACR utilizando una **Private IP**.

Ventajas:

- El tráfico permanece dentro de la red de Azure.
- No es necesario exponer el registro a Internet.

---

# 15. Dedicated Data Endpoint

El **Dedicated Data Endpoint** separa el tráfico de administración del tráfico de transferencia de imágenes.

Se utiliza principalmente en escenarios con:

- Firewalls.
- Private Endpoints.
- Reglas de red estrictas.

---

# 16. SKUs

Azure Container Registry dispone de tres niveles.

| SKU | Características |
|------|-----------------|
| **Basic** | Desarrollo |
| **Standard** | Producción |
| **Premium** | Geo-Replication, Private Link, mayor rendimiento |

---

# 17. Integración con Azure

ACR puede integrarse directamente con:

- AKS
- Azure Container Apps
- Azure Container Instances
- Azure App Service

No es necesario utilizar Docker Hub.

---

# 18. Buenas prácticas

Microsoft recomienda:

- Utilizar **Microsoft Entra ID** y Azure RBAC.
- Asignar el rol **AcrPush** únicamente a quienes publiquen imágenes.
- Utilizar **Private Endpoint** para entornos críticos.
- Habilitar **Content Trust** cuando se requieran imágenes firmadas.
- Utilizar **Geo-Replication** para aplicaciones globales.

---

# Preguntas trampa del AZ-104

✅ Antes de ejecutar **docker push** debe ejecutarse **az acr login**.

✅ Antes del Push es obligatorio realizar un **docker tag** utilizando el **Login Server** del ACR.

✅ El **Login Server** tiene el formato:

```
<nombre>.azurecr.io
```

✅ **Content Trust** garantiza el uso de imágenes firmadas.

✅ El rol necesario para publicar imágenes es **AcrPush**.

✅ **AcrPull** únicamente permite descargar imágenes.

✅ **Geo-Replication** solo está disponible en el **SKU Premium**.

✅ **Private Endpoint** proporciona una **Private IP** al registro.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Container Registry | Registro privado de imágenes |
| Repository | Agrupa imágenes |
| Tag | Identifica versiones |
| Login Server | `<nombre>.azurecr.io` |
| az acr login | Autenticación |
| docker tag | Etiquetar imagen |
| docker push | Publicar imagen |
| docker pull | Descargar imagen |
| Content Trust | Imágenes firmadas |
| AcrPull | Solo lectura |
| AcrPush | Publicar imágenes |
| Geo-Replication | Solo Premium |
| Private Endpoint | Acceso mediante Private IP |
| Servicios compatibles | AKS, ACI, ACA y App Service |
