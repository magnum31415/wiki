[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Container Registry (ACR) y Azure Container Instances (ACI) - Resumen AZ-104


# Índice

- [Azure Container Registry (ACR) y Azure Container Instances (ACI) - Resumen AZ-104](#azure-container-registry-acr-y-azure-container-instances-aci---resumen-az-104)
- [Azure Container Registry (ACR)](#azure-container-registry-acr)
  - [Qué es](#qué-es)
  - [Uso típico](#uso-típico)
  - [Estructura básica](#estructura-básica)
- [Azure Container Instances (ACI)](#azure-container-instances-aci)
  - [Qué es](#qué-es-1)
  - [Uso típico](#uso-típico-1)
- [Cómo funciona ACI con ACR](#cómo-funciona-aci-con-acr)
- [Requisitos para usar imágenes privadas](#requisitos-para-usar-imágenes-privadas)
- [Roles importantes ACR](#roles-importantes-acr)
- [AcrPull](#acrpull)
  - [Permite](#permite)
  - [NO permite](#no-permite)
- [Managed Identity](#managed-identity)
- [Configuración recomendada](#configuración-recomendada)
- [Private Endpoint](#private-endpoint)
  - [Qué es](#qué-es-2)
  - [Qué hace](#qué-hace)
  - [Qué NO hace](#qué-no-hace)
- [Concepto clave](#concepto-clave)
  - [Networking ≠ Authorization](#networking--authorization)
- [Error típico AZ-104](#error-típico-az-104)
  - [Problema](#problema)
  - [Solución habitual](#solución-habitual)
- [Dedicated Data Endpoint](#dedicated-data-endpoint)
  - [Qué es](#qué-es-3)
  - [Sin dedicated endpoint](#sin-dedicated-endpoint)
  - [Con dedicated endpoint](#con-dedicated-endpoint)
  - [Qué mejora realmente](#qué-mejora-realmente)
  - [Qué NO hace](#qué-no-hace-1)
- [Lo que normalmente falta](#lo-que-normalmente-falta)
- [Diferencia importante](#diferencia-importante)
- [Data Plane vs Management Plane](#data-plane-vs-management-plane)
  - [Data Plane](#data-plane)
  - [Management Plane](#management-plane)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
  - [Trampa 1](#trampa-1)
  - [Trampa 2](#trampa-2)
  - [Trampa 3](#trampa-3)
  - [Trampa 4](#trampa-4)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave examen](#frases-clave-examen)
- [Quién debe tener AcrPull](#quién-debe-tener-acrpull)
  - [Concepto clave](#concepto-clave-1)
  - [Qué NO debe recibir AcrPull](#qué-no-debe-recibir-acrpull)
  - [Qué identidad sí necesita AcrPull](#qué-identidad-sí-necesita-acrpull)
  - [ACR Tasks](#acr-tasks)
  - [Trampa típica examen](#trampa-típica-examen)
  - [Regla importante](#regla-importante)


# Azure Container Registry (ACR)

## Qué es

Registro privado de imágenes Docker/OCI administrado por Azure.

---

## Uso típico

- Imágenes Docker
- AKS
- Azure Container Apps
- Azure Container Instances (ACI)
- CI/CD

---

## Estructura básica

```text
ACR
 ↓
Repository
 ↓
Image
 ↓
Tag
```

Ejemplo:

```text
corpregistry.azurecr.io/webappimage:v1
```

---

# Azure Container Instances (ACI)

## Qué es

Servicio serverless para ejecutar contenedores sin administrar VMs.

---

## Uso típico

- Testing
- Automatización
- Jobs rápidos
- Procesos batch

---

# Cómo funciona ACI con ACR

ACI necesita:

1. Llegar al ACR "Azure Container Registry"
2. Tener permisos para hacer pull

Flujo típico:

```text
ACI "Azure Container Instances"
 ↓
ACR "Azure Container Registry"
 ↓
Pull image
 ↓
Run container
```

---

# Requisitos para usar imágenes privadas

| Requisito | Obligatorio |
|---|---|
| Acceso red al ACR | ✅ |
| Permisos pull | ✅ |
| Imagen válida | ✅ |

---

# Roles importantes ACR

| Rol | Función |
|---|---|
| AcrPull | Descargar imágenes |
| AcrPush | Subir imágenes |
| Owner | Administración total |

---

# AcrPull

## Permite

✅ Descargar imágenes

## NO permite

❌ Push  
❌ Delete  
❌ Administrar ACR  

---

# Managed Identity

ACI puede usar:

| Tipo | Soportado |
|---|---|
| System-assigned MI | ✅ |
| User-assigned MI | ✅ |

---

# Configuración recomendada

```text
ACI
 ↓
Managed Identity
 ↓
AcrPull
 ↓
ACR
```

---

# Private Endpoint

## Qué es

Permite acceso privado mediante IP privada dentro de una VNet.

---

## Qué hace

✅ Conectividad privada  
✅ Network isolation  
✅ Private Link  

---

## Qué NO hace

❌ Dar permisos RBAC  
❌ Autorizar pull imágenes  
❌ Resolver autenticación  

---

# Concepto clave

## Networking ≠ Authorization

| Concepto | Función |
|---|---|
| Private Endpoint | Conectividad |
| AcrPull | Permisos |

---

# Error típico AZ-104

## Problema

```text
ACI cannot pull image from ACR
```

## Solución habitual

Asignar:

```text
AcrPull
```

a la Managed Identity del ACI.

---

# Dedicated Data Endpoint

## Qué es

Separa tráfico de:

| Tipo tráfico | Función |
|---|---|
| Management plane | Administrar ACR |
| Data plane | Pull/Push imágenes |

---

# Sin dedicated endpoint

```text
registry.azurecr.io
```

---

# Con dedicated endpoint

| Endpoint | Uso |
|---|---|
| registry.azurecr.io | Management |
| registry.region.data.azurecr.io | Data plane |

---

# Qué mejora realmente

✅ Seguridad  
✅ Network isolation  
✅ Firewall granular  
✅ Routing/networking  

---

# Qué NO hace

❌ NO da permisos  
❌ NO asigna AcrPull  
❌ NO corrige autenticación  
❌ NO arregla tags incorrectos  

---

# Lo que normalmente falta

| Problema | Solución |
|---|---|
| Falta permisos pull | AcrPull |
| Sin Managed Identity | Crear MI |
| Password incorrecto | Corregir credenciales |
| Imagen/tag incorrecto | Corregir image/tag |
| Firewall | Configurar networking |

---

# Diferencia importante

| Feature | Función |
|---|---|
| Private Endpoint | Acceso privado |
| Dedicated Data Endpoint | Separar data plane |
| AcrPull | Permisos pull |

---

# Data Plane vs Management Plane

## Data Plane

Operaciones sobre imágenes:

- Pull
- Push
- Layers
- Manifests

---

## Management Plane

Administrar recurso Azure:

- Crear ACR
- Configurar SKU
- Configurar networking

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que Private Endpoint da permisos.

❌ Incorrecto

---

## Trampa 2

Pensar que networking basta.

❌ Incorrecto

También necesitas:

✅ AcrPull  
✅ RBAC  

---

## Trampa 3

Confundir AcrPull y AcrPush.

| Rol | Función |
|---|---|
| AcrPull | Descargar |
| AcrPush | Subir |

---

## Trampa 4

Pensar que Dedicated Data Endpoint resuelve autenticación.

❌ Incorrecto

Solo mejora networking.

---

# Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Descargar imágenes ACR | AcrPull |
| Ejecutar contenedor | ACI |
| Registry privado | ACR |
| Acceso privado ACR | Private Endpoint |
| Separar tráfico data plane | Dedicated Data Endpoint |

---

# Reglas rápidas AZ-104

```text
Private Endpoint = networking
```

```text
AcrPull = authorization
```

```text
Dedicated Data Endpoint = data plane isolation
```

---

# Frases clave examen

```text
ACI requires permission to pull images from ACR.
```

```text
A private endpoint does not grant image pull permissions.
```

```text
Dedicated data endpoints improve networking isolation, not authorization.
```
# Quién debe tener AcrPull

## Concepto clave

El rol:

```text
AcrPull
```

debe asignarse a:

```text
la identidad que realmente hace el pull de la imagen
```

NO a cualquier recurso relacionado con ACR.

---

# Qué NO debe recibir AcrPull

Ejemplo incorrecto:

```text
AcrPull → ACR-Tasks-Network
```

❌ Incorrecto

Porque:

```text
ACR-Tasks-Network NO descarga la imagen para ACI.
```

---

# Qué identidad sí necesita AcrPull

En Azure Container Instances normalmente el pull lo hace:

- Managed Identity del ACI
- Service Principal configurado
- Credenciales del registry

---

# Configuración correcta típica

```text
ACI
 ↓
Managed Identity
 ↓
AcrPull
 ↓
ACR
```

---

# ACR Tasks

## Qué es

Servicio de automatización/build dentro de Azure Container Registry.

---

## Uso típico

- Build imágenes
- Automatización CI/CD
- Tareas automáticas ACR

---

## Importante

ACR Tasks:

❌ NO ejecuta Azure Container Instances  
❌ NO hace el pull para ACI  

---

# Trampa típica examen

Microsoft intenta que pienses:

```text
Cualquier identidad relacionada con ACR sirve
```

❌ Incorrecto

RBAC debe asignarse:

```text
al principal que ejecuta la acción
```

---

# Ejemplo importante

## Acción

```text
Pull image
```

## Principal correcto

✅ Azure Container Instance identity

## Principal incorrecto

❌ ACR-Tasks-Network

---

# Regla importante

```text
The identity performing the image pull must have the AcrPull role.
```

---

# Tabla resumen

| Recurso | Función | Necesita AcrPull |
|---|---|---|
| ACI | Ejecuta contenedor | ✅ |
| AKS kubelet identity | Pull imágenes | ✅ |
| Container Apps | Pull imágenes | ✅ |
| ACR Tasks | Build automation | ❌ normalmente |
| ACR-Tasks-Network | Networking interno ACR Tasks | ❌ |

---

# Frase clave examen

```text
Assign AcrPull to the identity that pulls the container image.
```
