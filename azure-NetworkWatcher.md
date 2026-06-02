[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Network Watcher

## ¿Qué es Network Watcher?

Azure Network Watcher es un servicio regional de Azure que proporciona herramientas de monitorización, diagnóstico y captura de información de red.

- No enruta tráfico.
- No actúa como firewall.
- No es una VNet.
- No es un appliance.

Es un servicio de gestión que permite analizar y diagnosticar recursos de red de Azure.

---

## ¿Para qué sirve?

Permite realizar tareas de troubleshooting y observabilidad de red.

### Ejemplos

```text
- VNet Flow Logs
- Packet Capture
- Connection Monitor
- Next Hop
- IP Flow Verify
- Topology
- VPN Troubleshooting
```

---

## ¿Dónde se despliega?

Network Watcher es un recurso regional.

Existe un Network Watcher por región.

Ejemplo:

```text
Subscription
│
├── Germany West Central
│   └── NetworkWatcher_germanywestcentral
│
└── Sweden Central
    └── NetworkWatcher_swedencentral
```

---

## ¿De dónde cuelga?

Pertenece a una suscripción Azure.

Normalmente Microsoft lo crea dentro del Resource Group:

```text
NetworkWatcherRG
```

Ejemplo:

```text
Connectivity Subscription
│
└── NetworkWatcherRG
     ├── NetworkWatcher_germanywestcentral
     └── NetworkWatcher_swedencentral
```

Tipo de recurso:

```text
Microsoft.Network/networkWatchers
```

---

## Relación con las VNets

- Network Watcher no forma parte de una VNet.
- No está dentro de una VNet.
- No depende de una VNet concreta.

Ejemplo:

```text
Connectivity Subscription
│
├── NetworkWatcher_germanywestcentral
│
├── Hub VNet
├── SAP VNet
├── CRM VNet
└── AI VNet
```

Un único Network Watcher puede gestionar múltiples VNets de la misma región.

---

## Relación con VNet Flow Logs

Network Watcher es el servicio que permite habilitar los Flow Logs.

```text
Network Watcher
        │
        ▼
VNet Flow Logs
        │
        ▼
Storage Account
        │
        ▼
      SIEM
```

Sin Network Watcher no se pueden habilitar VNet Flow Logs.

---

## Principales funcionalidades

### VNet Flow Logs

Captura los flujos de tráfico IP de una VNet.

Ejemplo:

```text
VM1
 │
 ▼
VM2
```

Información registrada:

```text
IP origen
IP destino
Puerto origen
Puerto destino
TCP / UDP
Bytes transferidos
Estado del flujo
```

---

### Connection Monitor

Verifica conectividad entre recursos.

Ejemplo:

```text
VM1
 │
 ▼
VM2
```

Permite saber:

```text
- Latencia
- Disponibilidad
- Pérdida de paquetes
```

---

### Packet Capture

Captura paquetes directamente desde una máquina virtual.

Equivalente a:

```text
tcpdump
Wireshark
```

pero gestionado desde Azure.

---

### IP Flow Verify

Permite comprobar si un flujo será permitido o bloqueado.

Ejemplo:

```text
VM
 │
 ▼
Puerto TCP 443
```

Resultado:

```text
Allow
```

o

```text
Deny
```

incluyendo la regla responsable.

---

### Next Hop

Permite determinar la siguiente ruta utilizada.

Ejemplo:

```text
VM
 │
 ▼
Internet
```

Resultado:

```text
Azure Firewall
```

o

```text
Internet
```

o

```text
Virtual Network Gateway
```

---

### Topology

Genera automáticamente un mapa de red.

Ejemplo:

```text
VNet
│
├── Subnet Frontend
├── Subnet Backend
├── NSG
├── Route Table
└── VM
```

---

## Ejemplo en una Azure Landing Zone

```text
Connectivity Subscription
│
├── NetworkWatcherRG
│
├── NetworkWatcher_germanywestcentral
│
├── NetworkWatcher_swedencentral
│
├── Hub-GWC
├── Hub-SWC
├── VPN Gateway
├── ExpressRoute Gateway
└── Azure Firewall
```

---

## Ejemplo con VNet Flow Logs

```text
SAP VNet
      │
      ▼
VNet Flow Logs
      │
      ▼
Network Watcher
      │
      ▼
Storage Account
      │
      ▼
Corporate SIEM
```

---

## ¿Tiene coste?

El recurso Network Watcher no suele generar costes significativos por sí mismo.

Los costes aparecen al utilizar funcionalidades como:

```text
- VNet Flow Logs
- Traffic Analytics
- Packet Capture Storage
- Log Analytics
```

---

## Buenas prácticas en Azure Landing Zone

### Connectivity Subscription

```text
Connectivity Subscription
│
├── NetworkWatcher_germanywestcentral
├── NetworkWatcher_swedencentral
├── st-netlogs-prod-gwc-001
└── st-netlogs-prod-swc-001
```

### Una instancia por región

```text
Germany West Central
    ↓
NetworkWatcher_germanywestcentral

Sweden Central
    ↓
NetworkWatcher_swedencentral
```

### Automatizar mediante Terraform

```text
Terraform
    ↓
Network Watcher
    ↓
VNet Flow Logs
    ↓
Storage Account
```

### Automatizar mediante Azure Policy

```text
Nueva VNet
     ↓
DeployIfNotExists
     ↓
Activar VNet Flow Logs
```

---

## Resumen

```text
Network Watcher
    ↓
Servicio regional de diagnóstico y monitorización de red

No transporta tráfico
No es una VNet
No es un firewall

Permite:
    - VNet Flow Logs
    - Packet Capture
    - Connection Monitor
    - Next Hop
    - IP Flow Verify
    - Topology

Normalmente:
    - Uno por región
    - Ubicado en Connectivity Subscription
    - Dentro de NetworkWatcherRG
```
