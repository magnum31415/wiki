[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Container Registry (ACR) y Azure Container Instances (ACI) - Resumen AZ-104


# Índice

- [Azure Container Registry (ACR) y Azure Container Instances (ACI) - Resumen AZ-104](#azure-container-registry-acr-y-azure-container-instances-aci---resumen-az-104)
- [Azure Container Registry (ACR)](#azure-container-registry-acr)
- [Azure Container Instances (ACI)](#azure-container-instances-aci)
- [Cómo funciona ACI con ACR](#cómo-funciona-aci-con-acr)
- [ACR Tasks](#acr-tasks)
- [Qué es Admin User](#qué-es-admin-user)
- [Container Group - CPU Requests vs CPU Limits](#container-group---cpu-requests-vs-cpu-limits)

---

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

## Requisitos para usar imágenes privadas

| Requisito | Obligatorio |
|---|---|
| Acceso red al ACR | ✅ |
| Permisos pull | ✅ |
| Imagen válida | ✅ |

---

## Roles importantes ACR

| Rol | Función |
|---|---|
| AcrPull | Descargar imágenes |
| AcrPush | Subir imágenes |
| Owner | Administración total |

---

## AcrPull

### Permite

✅ Descargar imágenes

### NO permite

❌ Push  
❌ Delete  
❌ Administrar ACR  

---

## Managed Identity

ACI puede usar:

| Tipo | Soportado |
|---|---|
| System-assigned MI | ✅ |
| User-assigned MI | ✅ |

---

## Configuración recomendada

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

## Private Endpoint

### Qué es

Permite acceso privado mediante IP privada dentro de una VNet.

---

### Qué hace

✅ Conectividad privada  
✅ Network isolation  
✅ Private Link  

---

### Qué NO hace

❌ Dar permisos RBAC  
❌ Autorizar pull imágenes  
❌ Resolver autenticación  

---

## Concepto clave

### Networking ≠ Authorization

| Concepto | Función |
|---|---|
| Private Endpoint | Conectividad |
| AcrPull | Permisos |

---

## Error típico AZ-104

### Problema

```text
ACI cannot pull image from ACR
```

### Solución habitual

Asignar:

```text
AcrPull
```

a la Managed Identity del ACI.

---

## Dedicated Data Endpoint

### Qué es

Separa tráfico de:

| Tipo tráfico | Función |
|---|---|
| Management plane | Administrar ACR |
| Data plane | Pull/Push imágenes |

---

## Sin dedicated endpoint

```text
registry.azurecr.io
```

---

## Con dedicated endpoint

| Endpoint | Uso |
|---|---|
| registry.azurecr.io | Management |
| registry.region.data.azurecr.io | Data plane |

---

## Qué mejora realmente

✅ Seguridad  
✅ Network isolation  
✅ Firewall granular  
✅ Routing/networking  

---

## Qué NO hace

❌ NO da permisos  
❌ NO asigna AcrPull  
❌ NO corrige autenticación  
❌ NO arregla tags incorrectos  

---

## Lo que normalmente falta

| Problema | Solución |
|---|---|
| Falta permisos pull | AcrPull |
| Sin Managed Identity | Crear MI |
| Password incorrecto | Corregir credenciales |
| Imagen/tag incorrecto | Corregir image/tag |
| Firewall | Configurar networking |

---

## Diferencia importante

| Feature | Función |
|---|---|
| Private Endpoint | Acceso privado |
| Dedicated Data Endpoint | Separar data plane |
| AcrPull | Permisos pull |

---

## Data Plane vs Management Plane

### Data Plane

Operaciones sobre imágenes:

- Pull
- Push
- Layers
- Manifests

---

### Management Plane

Administrar recurso Azure:

- Crear ACR
- Configurar SKU
- Configurar networking

---

## Trampas típicas AZ-104

### Trampa 1

Pensar que Private Endpoint da permisos.

❌ Incorrecto

---

### Trampa 2

Pensar que networking basta.

❌ Incorrecto

También necesitas:

✅ AcrPull  
✅ RBAC  

---

### Trampa 3

Confundir AcrPull y AcrPush.

| Rol | Función |
|---|---|
| AcrPull | Descargar |
| AcrPush | Subir |

---

### Trampa 4

Pensar que Dedicated Data Endpoint resuelve autenticación.

❌ Incorrecto

Solo mejora networking.

---

## Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Descargar imágenes ACR | AcrPull |
| Ejecutar contenedor | ACI |
| Registry privado | ACR |
| Acceso privado ACR | Private Endpoint |
| Separar tráfico data plane | Dedicated Data Endpoint |

---

## Reglas rápidas AZ-104

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

## Frases clave examen

```text
ACI requires permission to pull images from ACR.
```

```text
A private endpoint does not grant image pull permissions.
```

```text
Dedicated data endpoints improve networking isolation, not authorization.
```
## Quién debe tener AcrPull

### Concepto clave

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

## Qué NO debe recibir AcrPull

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

## Qué identidad sí necesita AcrPull

En Azure Container Instances normalmente el pull lo hace:

- Managed Identity del ACI
- Service Principal configurado
- Credenciales del registry

---

## Configuración correcta típica

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

## Trampa típica examen

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

## Ejemplo importante

## Acción

```text
Pull image
```

### Principal correcto

✅ Azure Container Instance identity

### Principal incorrecto

❌ ACR-Tasks-Network

---

## Regla importante

```text
The identity performing the image pull must have the AcrPull role.
```

---

## Tabla resumen

| Recurso | Función | Necesita AcrPull |
|---|---|---|
| ACI | Ejecuta contenedor | ✅ |
| AKS kubelet identity | Pull imágenes | ✅ |
| Container Apps | Pull imágenes | ✅ |
| ACR Tasks | Build automation | ❌ normalmente |
| ACR-Tasks-Network | Networking interno ACR Tasks | ❌ |

---

## Frase clave examen

```text
Assign AcrPull to the identity that pulls the container image.
```
---

# Qué es Admin User

`Admin User` es una funcionalidad de Azure Container Registry (ACR) que habilita autenticación mediante:

- Username
- Password

similar a un login clásico Docker Registry.

---

## Qué ocurre al habilitarlo

Cuando activas:

```text
Admin User = Enabled
```

Azure genera automáticamente:

| Credencial | Disponible |
|---|---|
| Username | ✅ |
| Password1 | ✅ |
| Password2 | ✅ |

---

## Dónde se ve

En Azure Portal:

```text
Azure Container Registry
    ↓
Settings
    ↓
Access Keys
```

---

## Para qué sirve

Permite autenticarse contra ACR usando credenciales tradicionales.

---

## Uso típico

- Docker login manual
- Azure Container Instances (ACI)
- Scripts
- Testing rápido
- Integraciones legacy
- Herramientas externas

---

## Ejemplo Docker Login

```bash
docker login corpregistry.azurecr.io
```

Azure pedirá:

```text
Username
Password
```

---

## Ejemplo completo

```bash
docker login corpregistry.azurecr.io \
  --username corpregistry \
  --password XXXXX
```

---

## Qué permite

Con las credenciales Admin User puedes:

✅ Pull imágenes  
✅ Push imágenes  
✅ Autenticación Docker clásica  

---

## Dónde puede usarse

| Servicio | Uso típico |
|---|---|
| Docker CLI | `docker login` |
| Azure Container Instances | Pull imágenes |
| Kubernetes | imagePullSecrets |
| CI/CD pipelines | Pull/Push |
| Scripts automatizados | Login ACR |

---

## Flujo típico

```text
ACI / Docker / AKS
 ↓
Username + Password
 ↓
ACR
 ↓
Pull image
```

---

## Diferencia con Managed Identity

| Método                  | Networking | Auth | Recomendado      |
| ----------------------- | ---------- | ---- | ---------------- |
| Private Endpoint        | ✅          | ❌    | Sí               |
| Dedicated Data Endpoint | ✅          | ❌    | Sí               |
| AcrPull + MI            | ❌          | ✅    | ✅ Mejor práctica |
| Admin User              | ❌          | ✅    | ⚠️ Menos seguro  |


### Admin User

Usa:

```text
username/password
```

---

### Managed Identity

Usa:

```text
Azure RBAC + Entra ID
```

---

## Comparativa importante

| Método | Tipo autenticación |
|---|---|
| Admin User | Username/Password |
| Managed Identity | RBAC + Identity |
| Service Principal | App registration |

---

## Ventajas de Admin User

| Ventaja | Explicación |
|---|---|
| Fácil configuración | Muy simple |
| Compatible Docker estándar | Sí |
| Compatible herramientas externas | Sí |
| Rápido para testing | Sí |

---

## Desventajas

| Problema | Explicación |
|---|---|
| Passwords estáticos | Sí |
| Shared credentials | Sí |
| Menos seguro | Sí |
| No RBAC granular | Sí |
| Difícil rotación | Sí |

---

## Best Practice Azure

Microsoft normalmente recomienda:

✅ Managed Identity  
✅ Service Principal  

antes que:

⚠️ Admin User

---

## Cuándo suele usarse

### Recomendado para

- Laboratorios
- Testing
- Pruebas rápidas
- Herramientas legacy
- Casos simples

---

### No recomendado para

- Producción crítica
- Entornos enterprise
- Zero Trust
- Seguridad avanzada

---

## Relación con AcrPull

### Admin User

NO necesita:

```text
AcrPull
```

porque usa autenticación clásica.

---

### Managed Identity

SÍ necesita:

```text
AcrPull
```

---

## Comparativa completa

| Método | Necesita AcrPull | Usa password |
|---|---|---|
| Admin User | ❌ | ✅ |
| Managed Identity | ✅ | ❌ |
| Service Principal | Puede usar RBAC | ✅ |

---

## Trampas típicas AZ-104

### Trampa 1

Pensar que Admin User solo sirve para administración.

❌ Incorrecto

También sirve para:

✅ Pull/Push imágenes

---

### Trampa 2

Pensar que Admin User usa RBAC.

❌ Incorrecto

Usa:

```text
username/password
```

---

### Trampa 3

Pensar que si no es best practice no funciona.

❌ Incorrecto

Funciona perfectamente.

---

## Casos típicos examen

| Escenario | Solución válida |
|---|---|
| ACI no puede hacer pull | Enable Admin User |
| Docker login manual | Admin User |
| AKS legacy pull secret | Admin User |
| Testing rápido | Admin User |

---

## Regla rápida examen

```text
Admin User provides username/password authentication for ACR.
```

---

## Frases clave AZ-104

```text
Enabling Admin User generates credentials for Docker authentication.
```

```text
Admin User can be used by ACI to pull container images.
```

```text
Managed Identity is more secure than Admin User.
```

---

# Container Group - CPU Requests vs CPU Limits

## Qué es un Container Group

En Azure Container Instances (ACI), un Container Group permite ejecutar varios contenedores juntos compartiendo:

- Red
- Storage
- Recursos CPU/memoria

---

# CPU Request

## Qué es

Cantidad de CPU garantizada para el contenedor.

---

# Ejemplo

| Contenedor | CPU request |
|---|---|
| cont1 | 2 CPUs |
| cont2 | 3 CPUs |

---

# Significado

```text
cont1 necesita 2 CPUs garantizadas
cont2 necesita 3 CPUs garantizadas
```

---

## CPU Limit

### Qué es

Cantidad máxima de CPU que el contenedor puede consumir.

---

### Ejemplo

| Contenedor | CPU request | CPU limit |
|---|---|---|
| cont2 | 3 | 6 |

---

### Significado

```text
cont2 necesita 3 CPUs
pero podría intentar consumir hasta 6 CPUs
```

---

### Problema de los limits altos

Si un contenedor puede consumir más CPU de la necesaria:

❌ Puede afectar a otros contenedores del mismo group.

---

### Qué ocurre si no defines limit

En Azure Container Instances:

```text
si no existe CPU limit
→ limit = request
```

---

### Resultado

| Contenedor | Request | Limit eliminado | Máximo real |
|---|---|---|---|
| cont1 | 2 | Sí | 2 CPUs |
| cont2 | 3 | Sí | 3 CPUs |

---

### Ventaja

Así:

✅ cont2 obtiene las 3 CPUs que necesita  
✅ no puede excederlas  
✅ no afecta a cont1  

### Regla importante AZ-104

```text
CPU limit must be greater than or equal to CPU request.
```

---

### Regla importante ACI

```text
If no limit is specified, the limit equals the request.
```

---

### Diferencia importante

| Concepto | Significado |
|---|---|
| CPU request | CPU garantizada |
| CPU limit | CPU máxima permitida |

---

### Trampa típica AZ-104

Muchos candidatos piensan:

```text
más limit = mejor rendimiento
```

❌ Incorrecto.

Puede provocar:

- Resource contention
- Impacto otros contenedores

---

### Tabla resumen examen

| Configuración | Resultado |
|---|---|
| request = 3, limit = 6 | Puede consumir hasta 6 |
| request = 3, sin limit | Máximo = 3 |
| limit < request | ❌ Inválido |

---

### Frases clave AZ-104

```text
CPU request defines guaranteed CPU resources.
```

```text
CPU limit defines maximum CPU usage.
```

```text
If no limit is specified, the limit equals the request.
```

