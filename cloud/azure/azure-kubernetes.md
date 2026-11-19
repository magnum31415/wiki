[Azure](azure.md)

# 📑 Índice



## 🐳 Azure Kubernetes & Contenedores

- [Azure Kubernetes](#azure-kubernetes)
- [Azure Container Registry (ACR)](#azure-container-registry-acr)
- [Azure Kubernetes Service (AKS)](#azure-kubernetes-service-aks)

---

## 📦 Azure Container Registry (ACR)

- [Tiers de Azure Container Registry](#-tiers-de-azure-container-registry)
- [Diferencia conceptual rápida](#-diferencia-conceptual-rápida)
- [¿Qué es Geo-replication?](#-qué-es-geo-replication)
- [Seguridad en ACR](#-seguridad-en-acr)
- [Preguntas típicas de examen](#-preguntas-típicas-de-examen)
- [Resumen ultra rápido examen](#-resumen-ultra-rápido-examen)

---

## ☸ Azure Kubernetes Service (AKS)

- [Escalado en AKS](#-1️⃣-escalado-autoscaling)
- [Cluster Autoscaler](#cluster-autoscaler)
- [HPA (Horizontal Pod Autoscaler)](#-1️⃣-escalado-autoscaling)
- [KEDA](#-1️⃣-escalado-autoscaling)
- [VPA (Vertical Pod Autoscaler)](#-1️⃣-escalado-autoscaling)
- [Virtual Nodes](#-1️⃣-escalado-autoscaling)
- [Azure Arc-enabled Kubernetes](#-2️⃣-gobernanza-y-gestión-híbrida)

---

## 🚀 Flujo CI/CD y ejecución

- [Flujo típico CI/CD](#-flujo-típico-cicd)
- [¿Dónde se ejecutan las imágenes?](#vas-a-ejecutar-contenedores-en-azure)

##  CNI (Container Networking Interface)

- [🌐 CNI (Container Networking Interface)](#-cni-container-networking-interface)
- [🎯 ¿Qué problema resuelve?](#-qué-problema-resuelve)
- [🧠 ¿Cómo funciona?](#-cómo-funciona)
- [🧩 Componentes clave](#-componentes-clave)
- [☸️ CNI en Kubernetes](#️-cni-en-kubernetes)
- [☁️ CNI en Azure (AKS)](#️-cni-en-azure-aks)
  - [1️⃣ Azure CNI](#1️⃣-azure-cni)
  - [2️⃣ Kubenet (legacy)](#2️⃣-kubenet-legacy)
- [🔎 Diferencia importante](#-diferencia-importante)
- [📌 Resumen rápido](#-resumen-rápido)




# Azure Kubernetes 
## ¿Vas a ejecutar contenedores en Azure?
````
¿Vas a ejecutar contenedores en Azure?
│
└── Sí
    │
    ├── ¿Necesitas orquestación Kubernetes gestionada?
    │       │
    │       └── Azure Kubernetes Service (AKS)
    │               │
    │               ├── ¿De dónde obtienen las imágenes los pods?
    │               │
    │               │       └── Azure Container Registry (ACR)
    │               │               │
    │               │               ├── Función:
    │               │               │       Registro privado de imágenes Docker/OCI
    │               │               │
    │               │               ├── Flujo típico:
    │               │               │       1️⃣ Dev hace build
    │               │               │       2️⃣ Push a ACR
    │               │               │       3️⃣ AKS hace pull desde ACR
    │               │               │
    │               │               ├── Integración recomendada:
    │               │               │       az aks update --attach-acr
    │               │               │       (Managed Identity)
    │               │               │
    │               │               ├── Seguridad:
    │               │               │       RBAC Azure
    │               │               │       Managed Identity
    │               │               │       Private Link (Premium)
    │               │               │
    │               │               └── ¿Qué SKU elegir?
    │               │                       Dev/Test → Basic
    │               │                       Producción → Standard
    │               │                       Multi-región / Enterprise → Premium
    │               │
    │               ├── Escalado en AKS (Kubernetes-native):
    │               │       Horizontal Pod Autoscaler (HPA)
    │               │           → Escala número de pods según CPU/métricas
    │               │
    │               │       Vertical Pod Autoscaler (VPA)
    │               │           → Ajusta CPU/RAM de los pods 
    │               │
    │               │       Cluster Autoscaler
    │               │           → Escala nodos "Añadir más máquinas virtuales al cluster" del cluster (VMSS) -
    │               │
    │               │       Kubernetes Event-Driven Autoscaling (KEDA)
    │               │           → Escala pods "Crear más copias de tu aplicación" por eventos (Service Bus, Kafka, etc.)
    │               │
    │               ├── Escalado a nivel Azure (fuera de Kubernetes):
    │               │
    │               │       Azure Monitor Autoscale
    │               │           → Escala recursos Azure basados en métricas
    │               │           → Se aplica a:
    │               │               - VM Scale Sets
    │               │               - App Service Plans
    │               │               - Azure Container Apps
    │               │           → Basado en CPU, memoria, colas, métricas personalizadas
    │               │
    │               │       ⚠ En AKS:
    │               │           No escala pods.
    │               │           Se usa indirectamente al escalar VM Scale Sets.
    │               │
    │               ├── Alta disponibilidad:
    │               │       AKS multi-zone
    │               │       ACR Premium → Geo-replication
    │               │
    │               └── Escenario ideal conjunto:
    │                       Microservicios
    │                       CI/CD pipelines
    │                       Arquitectura cloud-native
    │
    └── No Kubernetes
            │
            ├── Azure Container Apps
            │       ├── Usa KEDA internamente
            │       ├── Puede usar ACR
            │       └── Escala automáticamente (HTTP o eventos)
            │
            ├── Azure App Service (contenedores)
            │       ├── Puede usar ACR
            │       └── Escala con Azure Monitor Autoscale
            │
            └── Azure Functions (contenedores)
                    ├── Puede usar ACR
                    ├── Escala automática integrada
                    └── Puede usar Azure Monitor Autoscale en plan dedicado

````
## Azure Container Registry (ACR)



````
¿Vas a ejecutar contenedores en Azure?
│
└── Sí
    │
    ├── Necesitas un registro privado de imágenes
    │       │
    │       └── Azure Container Registry (ACR)
    │               │
    │               ├── Función: Registro privado de imágenes Docker/OCI
    │               │
    │               ├── Flujo típico:
    │               │       1️⃣ Dev hace build
    │               │       2️⃣ Push a ACR
    │               │       3️⃣ AKS hace pull desde ACR
    │               │
    │               ├── Integración recomendada:
    │               │       az aks update --attach-acr
    │               │       (Permite a AKS autenticarse con ACR vía Managed Identity)
    │               │
    │               ├── Seguridad:
    │               │       RBAC Azure
    │               │       Managed Identity
    │               │       Private Link (Premium)
    │               │
    │               └── ¿Qué SKU elegir?
    │               │        Dev/Test → Basic
    │               │        Producción → Standard
    │               │        Multi-región / Enterprise → Premium
    │               │
    │               ├── Basic SKU
    │               │       ├── Coste bajo
    │               │       ├── Throughput limitado
    │               │       ├── Sin geo-replication
    │               │       ├── Sin Private Link
    │               │       └── Escenario ideal:
    │               │               Desarrollo
    │               │               Laboratorios
    │               │               Workloads pequeños
    │               │
    │               ├── Standard SKU
    │               │       ├── Más almacenamiento
    │               │       ├── Mayor throughput
    │               │       ├── Webhooks
    │               │       ├── Sin geo-replication
    │               │       └── Escenario ideal:
    │               │               Producción pequeña/mediana
    │               │               Equipos CI/CD
    │               │
    │               └── Premium SKU
    │                       ├── Geo-replication multi-región
    │                       ├── Private Link
    │                       ├── Mayor throughput
    │                       ├── Content Trust
    │                       ├── Zone redundancy
    │                       └── Escenario ideal:
    │                               Enterprise
    │                               Multi-región
    │                               Alta seguridad
    │
    │
    ├── ¿Dónde se ejecutan las imágenes?
    │       │
    │       ├── Azure Kubernetes Service (AKS)
    │       │       │
    │       │       ├── AKS hace pull de imágenes desde ACR
    │       │       ├── Integración recomendada:
    │       │       │       Managed Identity + attach-acr
    │       │       │
    │       │       ├── Escalado:
    │       │       │       HPA → Escala pods
    │       │       │       Cluster Autoscaler → Escala nodos
    │       │       │       KEDA → Escala por eventos
    │       │       │
    │       │       └── Alta disponibilidad:
    │       │               AKS multi-zone
    │       │               ACR Premium → Geo-replication
    │       │
    │       ├── Azure Container Apps
    │       ├── Azure App Service (contenedores)
    │       └── Azure Functions (contenedores)
    │       │
    │       └── Escenario ideal conjunto:
    │             Microservicios
    │             CI/CD pipelines
    │             Arquitectura cloud-native
    │       │
    |       └── No Kubernetes
    |          │
    |          ├── Azure Container Apps → Puede usar ACR
    |          ├── Azure App Service (contenedores) → Puede usar ACR
    |          └── Azure Functions (contenedores) → Puede usar ACR
    │
    └── Flujo típico CI/CD
            │
            ├── Build imagen
            ├── Push a ACR
            └── Servicio (AKS/App Service/etc.) hace pull

````


## Azure Kubernetes Service (AKS)

| Qué escala                | Herramienta        |
| ------------------------- | ------------------ |
| Infraestructura (VMs)     | Cluster Autoscaler |
| Pods por métricas         | HPA                |
| Pods por eventos externos | KEDA               |
| Recursos internos del pod | VPA                |
| Sin gestionar nodos       | Virtual Nodes      |
| Clusters fuera de Azure   | Arc                |
 
````
Azure Kubernetes Service (AKS)
│
├── 🔹 1️⃣ Escalado (Autoscaling)
│   │
│   ├── ¿Necesitas escalar la infraestructura (VMs / nodos)?
│   │   │
│   │   └── ✅ Cluster Autoscaler
│   │         ├── Escala → Nodos (VMs del node pool)
│   │         ├── Tipo → Horizontal
│   │         ├── Cuándo → Pods pendientes / nodos infrautilizados
│   │         ├── Ideal para → Apps con picos variables
│   │         └── Nivel → Infraestructura
│   │
│   ├── ¿Necesitas escalar aplicaciones (pods)?
│   │   │
│   │   ├── Basado en métricas (CPU/memoria)?
│   │   │       └── ✅ HPA (Horizontal Pod Autoscaler)
│   │   │             ├── Escala → Número de pods
│   │   │             ├── Tipo → Horizontal
│   │   │             ├── Ideal → APIs, frontends, microservicios
│   │   │             └── Muy típico examen → "scale on CPU"
│   │   │
│   │   ├── Basado en eventos externos (cola, Kafka, Service Bus)?
│   │   │       └── ✅ KEDA
│   │   │             ├── Escala → Pods
│   │   │             ├── Tipo → Horizontal event-driven
│   │   │             ├── Ideal → Sistemas async / procesamiento de colas
│   │   │             └── Muy típico examen → "scale on queue length"
│   │   │
│   │   └── ¿Necesitas ajustar recursos internos del contenedor?
│   │           └── ✅ VPA (Vertical Pod Autoscaler)
│   │                 ├── Escala → CPU / memoria del pod
│   │                 ├── Tipo → Vertical
│   │                 ├── Puede reiniciar pods
│   │                 └── Ideal → Workloads estables / stateful
│   │
│   └── ¿Necesitas escalar sin añadir nodos VM?
│           └── ✅ Virtual Nodes (Azure Container Instances)
│                 ├── Ejecuta pods sin gestionar nodos
│                 ├── Ideal → Picos impredecibles / burst
│                 └── Nivel → Serverless extension de AKS
|                 │
|                 ├── Qué es:
|                 │       Extensión serverless de AKS
|                 │       Ejecuta pods directamente en ACI
|                 │       No usas nodos VM del cluster
|                 │
|                 ├── Ventaja principal:
|                 │       Escalado rápido para picos impredecibles (burst)
|                 │       Sin gestionar infraestructura
|                 │
|                 ├── Limitaciones importantes:
|                 │
|                 │       ❌ No soporta pods Windows → Solo contenedores Linux
|                 │       ❌ No soporta DaemonSets → No puedes ejecutar agentes por nodo
|                 │       ❌ No soporta privilegios elevados → No privileged containers, No acceso al host
|                 │       ❌ No soporta host networking
|                 │       ❌ No soporta storage persistente tipo Azure Disk (solo Azure Files)
|                 │       ❌ No es ideal para workloads stateful
|                 │       ❌ Sin soporte completo para:
|                 │           - Custom CNI avanzado
|                 │           - GPU
|                 │           - Windows containers
|                 │
|                 ├── Red:
|                 │       Usa Azure VNet
|                 │       Requiere configuración específica
|                 │
|                 ├── Cuándo usarlo:
|                 │       ✔ Jobs batch
|                 │       ✔ Procesamiento por eventos
|                 │       ✔ Picos temporales de carga
|                 │       ✔ Workloads stateless
|                 │
|                 └── Cuándo NO usarlo:
|                         ✖ Workloads Windows
|                         ✖ Apps stateful con Azure Disk
|                         ✖ Necesitas DaemonSets
|                         ✖ Necesitas control profundo de nodo
│
│
├── 🔹 2️⃣ Gobernanza y Gestión híbrida
│   │
│   └── ¿Necesitas gestionar clusters fuera de Azure?
│           └── ✅ Azure Arc-enabled Kubernetes
│                 ├── Gestiona clusters on-prem / otros clouds
│                 ├── Permite → Azure Policy, GitOps, Monitorización
│                 └── No es escalado → Es gobernanza
│
│
└── 🧠 Regla mental rápida
        │
        ├── Escalar nodos → Cluster Autoscaler
        ├── Escalar pods por CPU → HPA
        ├── Escalar pods por eventos → KEDA
        ├── Ajustar CPU/RAM del pod → VPA
        ├── Escalar sin VMs → Virtual Nodes
        └── Gestionar Kubernetes fuera de Azure → Arc

````
### Cluster Autoscaler

# 📦 Azure Container Registry (ACR) – Tiers y Resumen Examen

Azure Container Registry es el servicio privado de Azure para almacenar imágenes Docker y artefactos OCI.

---

# 📊 Tiers de Azure Container Registry

| Característica | Basic | Standard | Premium |
|---------------|--------|----------|----------|
| Uso típico | Dev/Test | Producción pequeña | Producción empresarial |
| Webhooks | ❌ | ✅ | ✅ |
| Geo-replication | ❌ | ❌ | ✅ |
| Zone redundancy | ❌ | ❌ | ✅ |
| Private Link | ❌ | ❌ | ✅ |
| Network rules (Firewall) | ❌ | ❌ | ✅ |
| Customer-managed keys (CMK) | ❌ | ❌ | ✅ |
| Content trust | ❌ | ❌ | ✅ |
| Mayor throughput | Bajo | Medio | Alto |
| SLA más alto | ❌ | ❌ | ✅ |

---

# 🧠 Diferencia conceptual rápida

## 🔹 Basic
- Para pruebas
- Sin características de red avanzada
- Sin replicación

## 🔹 Standard
- Añade webhooks
- Mejor rendimiento
- No tiene red privada avanzada

## 🔹 Premium
- Multi-región (Geo-replication)
- Integración con Private Link
- Seguridad avanzada
- Alto rendimiento
- Soporta escenarios empresariales

---

# 🌍 ¿Qué es Geo-replication?

Permite replicar el registry en múltiples regiones.

Beneficios:
- Baja latencia
- Alta disponibilidad regional
- Cumplimiento normativo

Solo disponible en **Premium**.

---

# 🔐 Seguridad en ACR

Opciones de autenticación:

- Azure AD (Microsoft Entra ID)
- Managed Identity
- Service Principal
- Admin user (no recomendado en producción)

---

# 🎯 Preguntas típicas de examen

### ❓ Necesitas replicar imágenes en varias regiones
→ Premium

### ❓ Necesitas acceso privado desde VNet
→ Premium (Private Link)

### ❓ Solo entorno dev simple
→ Basic

### ❓ Necesitas webhooks
→ Standard o Premium

---

# 📌 Resumen ultra rápido examen

| Si el requisito menciona… | Tier correcto |
|---------------------------|---------------|
| Multi-región | Premium |
| Private Endpoint | Premium |
| Firewall | Premium |
| Solo pruebas | Basic |
| Webhooks | Standard |

---

# 🧠 Regla mental

Basic = laboratorio  
Standard = producción simple  
Premium = enterprise real  

---

Si quieres, puedo añadir también la diferencia entre:

ACR vs Docker Hub vs Azure Container Instances  
que suele aparecer mezclado en escenarios AZ-305.

---

# 🔷 ¿Qué es Azure Workload Identity?

**Azure Workload Identity permite que un pod de AKS se autentique directamente en Microsoft Entra ID sin usar secretos ni credenciales almacenadas.**

**Azure Workload Identity = identidad segura por pod en AKS sin secretos.**

| Método                          | Identidad por pod | Usa secretos | Recomendado |
| ------------------------------- | ----------------- | ------------ | ----------- |
| Service Principal               | ❌                 | ✅            | No          |
| Managed Identity nodo           | ❌                 | ❌            | Limitado    |
| Azure AD Pod Identity (antiguo) | ⚠️                | ❌            | Deprecado   |
| **Azure Workload Identity**     | ✅                 | ❌            | ✅ Sí        |

## 🔎 ¿Qué problema resuelve?

Antes, en AKS:
- Se usaban Service Principals
- O Managed Identity del nodo
- O secretos en Kubernetes
Problemas:
- Credenciales almacenadas
- Todos los pods comparten identidad
- No hay granularidad por microservicio

Riesgo de seguridad

## ✅ Qué hace Azure Workload Identity

Permite:
- Asignar identidad Entra ID a cada pod
- Autenticación basada en federación OIDC
- Sin secretos almacenados
- Permisos RBAC independientes
Cada pod puede:
- Acceder a Key Vault
- Acceder a Cosmos DB
- Acceder a Storage
- Usar RBAC
- Generar logs auditables


````
Pod en AKS
   ↓
Service Account Kubernetes
   ↓
Token OIDC firmado por AKS
   ↓
Microsoft Entra ID (federation)
   ↓
Access token para recurso Azure

````

No hay:
- Password
- Secret
- Certificado almacenado

## 🔐 Ventajas clave

- Principio de mínimo privilegio
- Auditoría granular
- Sin credenciales hardcoded
- Integración nativa con Entra ID
- Seguridad cloud-native

##🎯 Cuándo usarlo (examen AZ-305)

Si ves:
- AKS
- Microservicios
- Acceso a Key Vault / Storage / Cosmos
- Least privilege
- No secrets
👉 Respuesta correcta: Azure Workload Identity


## 🧠 Respuesta clara

**Azure Workload Identity NO es un servicio independiente.**  
No es un rol.  
No es un recurso que creas directamente en el portal.

👉 Es un **mecanismo de federación de identidad** entre AKS y Microsoft Entra ID.

---

# 🔎 Entonces, ¿qué es realmente?

Es una **técnica de autenticación basada en OIDC (OpenID Connect Federation)** que permite que un pod en AKS obtenga un token de acceso de Microsoft Entra ID sin usar secretos.

No es un servicio como Key Vault o Cosmos DB.

---

# 🏗 Componentes reales que intervienen

Cuando implementas Azure Workload Identity, estás combinando:

## En AKS:
- OIDC issuer habilitado en el cluster
- Service Account de Kubernetes anotada

## En Microsoft Entra ID:
- Managed Identity (normalmente User Assigned)
- Federated Identity Credential asociada

## En Azure:
- Asignaciones RBAC sobre los recursos (Key Vault, Cosmos, etc.)

---

# 📦 Clasificación correcta

| ¿Qué es? | Respuesta |
|-----------|-----------|
| Servicio Azure independiente | ❌ No |
| Rol RBAC | ❌ No |
| Recurso con SKU | ❌ No |
| Técnica de autenticación | ✅ Sí |
| Mecanismo de federación OIDC | ✅ Sí |
| Feature integrada de AKS + Entra ID | ✅ Sí |

---

# 🔁 Flujo simplificado

````
Pod en AKS
↓
Service Account
↓
Token OIDC del cluster
↓
Federación con Entra ID
↓
Token de acceso para recurso Azure
````


No hay:
- Password
- Secret
- Certificado almacenado

---

# 🎯 Definición tipo examen

> Azure Workload Identity es un mecanismo de federación OIDC que permite a los pods de AKS autenticarse en Microsoft Entra ID usando una identidad administrada sin almacenar secretos.


# 🌐 CNI (Container Networking Interface)

**CNI (Container Networking Interface)** es una especificación estándar que define cómo se configuran las redes para contenedores en entornos como Kubernetes.

No es un producto, sino una **interfaz/estándar** que permite que diferentes plugins de red proporcionen conectividad a los contenedores.

---

# 🎯 ¿Qué problema resuelve?

Cuando se crea un contenedor, necesita:

- Una dirección IP
- Conectividad de red
- Reglas de enrutamiento
- Acceso a otros servicios

CNI define **cómo el runtime del contenedor (como containerd)** solicita a un plugin de red que configure todo eso automáticamente.

---

# 🧠 ¿Cómo funciona?

Cuando Kubernetes crea un Pod:

1. El runtime del contenedor invoca el plugin CNI.
2. El plugin:
   - Asigna una IP
   - Configura interfaces virtuales
   - Aplica reglas de routing
3. El Pod queda conectado a la red del clúster.

---

# 🧩 Componentes clave

| Componente | Función |
|------------|----------|
| Especificación CNI | Define cómo interactúan runtime y plugin |
| Plugin CNI | Implementa la red (Calico, Azure CNI, Flannel, etc.) |
| Runtime (containerd) | Llama al plugin cuando crea el contenedor |

---

# ☸️ CNI en Kubernetes

Kubernetes **no implementa red directamente**.  
Delegaa la red a plugins CNI.

Ejemplos comunes:

- Calico
- Flannel
- Cilium
- Azure CNI (en AKS)

---

# ☁️ CNI en Azure (AKS)

En Azure Kubernetes Service (AKS) existen dos modelos principales:

## 1️⃣ Azure CNI

- Cada Pod obtiene una IP de la VNet.
- Integración directa con red Azure.
- Ideal cuando necesitas:
  - Integración con recursos en la VNet
  - Control granular de red

## 2️⃣ Kubenet (legacy)

- Pods usan red interna.
- Se hace NAT hacia la VNet.
- Más simple pero menos flexible.

---

# 🔎 Diferencia importante

| Azure CNI | Kubenet |
|------------|---------|
| IP real en VNet | IP interna NAT |
| Más integración empresarial | Más simple |
| Requiere más IPs disponibles | Consume menos IPs |

---

# 📌 Resumen rápido

> CNI es el estándar que permite configurar la red de contenedores automáticamente en Kubernetes mediante plugins especializados.

En AKS, Azure CNI es la implementación empresarial más común.

