# Azure Kubernetes 

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
