[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
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
    │               │           → Escala nodos del cluster (VMSS)
    │               │
    │               │       Kubernetes Event-Driven Autoscaling (KEDA)
    │               │           → Escala pods por eventos (Service Bus, Kafka, etc.)
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
