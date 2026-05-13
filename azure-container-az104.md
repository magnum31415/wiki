[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Container Registry (ACR) y Azure Container Instances (ACI) - Teoría AZ-104

# Azure Container Registry (ACR)

# Qué es

Azure Container Registry (ACR) es un registro privado de imágenes Docker y OCI administrado por Azure.

---

# Uso típico

- Almacenar imágenes Docker
- Integración CI/CD
- Kubernetes (AKS)
- Azure Container Apps
- Azure Container Instances (ACI)

---

# Arquitectura básica

```text
Azure Container Registry
    ↓
Repository
    ↓
Image
    ↓
Tag
```

---

# Ejemplo

```text
corpregistry.azurecr.io/webappimage:v1
```

---

# Azure Container Instances (ACI)

# Qué es

Servicio serverless para ejecutar contenedores rápidamente sin administrar VMs.

---

# Uso típico

- Testing
- Jobs rápidos
- Automatización
- Contenedores simples
- Procesos batch

---

# Cómo funciona ACI con ACR

ACI necesita descargar imágenes desde ACR.

Flujo típico:

```text
ACI
 ↓
Azure Container Registry
 ↓
Pull image
 ↓
Run container
```

---

# Requisitos para usar imágenes privadas

ACI necesita:

| Requisito | Obligatorio |
|---|---|
| Acceso de red al ACR | ✅ |
| Permisos para pull | ✅ |
| Imagen válida | ✅ |

---

# Roles importantes en ACR

| Rol | Permiso |
|---|---|
| AcrPull | Descargar imágenes |
| AcrPush | Subir imágenes |
| Owner | Administración completa |

---

# AcrPull

# Qué permite

✅ Pull de imágenes

---

# Qué NO permite

❌ Push  
❌ Delete  
❌ Administrar registry  

---

# Managed Identity

ACI puede autenticarse usando:

| Tipo | Soportado |
|---|---|
| System-assigned Managed Identity | ✅ |
| User-assigned Managed Identity | ✅ |

---

# Flujo recomendado

```text
ACI
 ↓
Managed Identity
 ↓
Role: AcrPull
 ↓
ACR
```

---

# Private Endpoint

# Qué es

Permite acceder a un recurso Azure usando IP privada dentro de una VNet.

---

# Características

| Feature | Soportado |
|---|---|
| Acceso privado | ✅ |
| Evita Internet pública | ✅ |
| Usa Private Link | ✅ |

---

# Qué hace realmente

Un Private Endpoint:

✅ Mejora networking  
✅ Aísla tráfico  
✅ Usa IP privada  

---

# Qué NO hace

| Acción | Soportado |
|---|---|
| Dar permisos RBAC | ❌ |
| Autorizar pull imágenes | ❌ |
| Resolver autenticación | ❌ |
| Crear identidades | ❌ |

---

# Diferencia importante

# Networking ≠ Authorization

Muchos errores en AZ-104 vienen de confundir:

| Concepto | Función |
|---|---|
| Private Endpoint | Conectividad |
| AcrPull | Autorización |

---

# Error típico en despliegue ACI

## Problema habitual

```text
ACI cannot pull image from ACR
```

---

# Solución habitual

Asignar:

```text
AcrPull
```

a la Managed Identity del ACI.

---

# Ejemplo correcto

```text
ACI
 ↓
Managed Identity
 ↓
AcrPull role
 ↓
ACR
```

---

# Qué suele preguntar el examen

AZ-104 suele evaluar:

| Concepto | Muy importante |
|---|---|
| AcrPull | ✅ |
| Managed Identity | ✅ |
| Private Endpoint | ✅ |
| RBAC vs Networking | ✅ |
| ACR authentication | ✅ |

---

# Comparativa importante

| Feature | Private Endpoint | AcrPull |
|---|---|---|
| Conectividad privada | ✅ | ❌ |
| Seguridad red | ✅ | ❌ |
| Permiso pull imágenes | ❌ | ✅ |
| Autenticación ACR | ❌ | ✅ |

---

# Trampas típicas AZ-104

# Trampa 1

Pensar que Private Endpoint da permisos.

❌ Incorrecto

---

# Trampa 2

Pensar que tener acceso de red basta.

❌ Incorrecto

También necesitas:

✅ RBAC  
✅ AcrPull  

---

# Trampa 3

Olvidar Managed Identity.

❌ Muy común.

---

# Trampa 4

Confundir AcrPull con AcrPush.

| Rol | Función |
|---|---|
| AcrPull | Descargar imágenes |
| AcrPush | Subir imágenes |

---

# Tabla resumen importante

| Necesidad | Solución |
|---|---|
| Descargar imágenes desde ACR | AcrPull |
| Ejecutar contenedor | ACI |
| Registry privado | ACR |
| Acceso privado ACR | Private Endpoint |

---

# Regla rápida examen

```text
Private Endpoint provides connectivity, not authorization.
```

---

# Frases clave AZ-104

```text
ACI requires permission to pull images from ACR.
```

```text
The AcrPull role is commonly required for Azure Container Instances.
```

```text
A private endpoint does not grant permissions to pull container images.
```
