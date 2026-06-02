# vnet-flow-logs

# Virtual Network Flow Logs (VNFL)

## ¿Qué es Virtual Network Flow Logs?

Virtual Network Flow Logs (VNFL) es la evolución de NSG Flow Logs.

Permite registrar los flujos de red (IP flows) que circulan por una Virtual Network (VNet), una Subnet o una NIC, independientemente de que exista o no un NSG asociado.

Registra información como:

```text
- IP origen
- IP destino
- Puerto origen
- Puerto destino
- Protocolo (TCP/UDP)
- Sentido del tráfico (inbound/outbound)
- Bytes enviados y recibidos
- Estado del flujo
- Decisión de reglas de seguridad
```

Su objetivo es proporcionar:

- Visibilidad de tráfico
- Threat hunting
- Investigación forense
- Detección de movimientos laterales
- Alimentación de SIEMs
- Auditoría de comunicaciones

## Tabla comparativa

| Característica | NSG Flow Logs | Virtual Network Flow Logs |
|----------------|---------------|---------------------------|
| Estado del servicio | En retirada | Recomendado por Microsoft |
| Nuevo despliegue | No recomendado | Sí |
| Nivel de captura | NSG | VNet, Subnet o NIC |
| Requiere NSG | Sí | No |
| Tráfico sin NSG | No visible | Visible |
| Throughput (bytes) | Limitado | Mejorado |
| Estado de cifrado | No | Sí |
| Azure Virtual Network Manager | No | Sí |
| Escalabilidad | Menor | Mayor |
| Cobertura de red | Parcial | Más amplia |
| Integración futura Microsoft | No | Sí |
| Azure Landing Zone | No recomendado | Recomendado |

## Diferencia conceptual

### NSG Flow Logs

Antes con NSG Flow Logs

```text
Internet
    │
    ▼
 NSG
    │
    ▼
 VM
```

Solo registra/veía tráfico que atravesaba un NSG.




Si no hay NSG:

```text
Internet
    │
    ▼
 VM
```

No hay logs.

### Virtual Network Flow Logs

```text
Virtual Network
│
├── Subnet A
│    └── VM1
│
├── Subnet B
│    └── VM2
│
└── Subnet C
     └── App Gateway
```

Registra el tráfico de toda la VNet independientemente de dónde esté aplicado el NSG.

## Ejemplos que sí cubre VNFL

### Tráfico entre spokes

```text
Spoke A VM
      │
      ▼
Spoke B VM
```

### Tráfico hacia Azure Firewall

```text
VM
 │
 ▼
Azure Firewall
 │
 ▼
Internet
```

### Tráfico hacia VPN Gateway

```text
On-Prem
   │
VPN
   │
VPN Gateway
   │
VNet
```

### Tráfico hacia Application Gateway

```text
Internet
   │
App Gateway
   │
Backend VM
```

## Casos de uso en una Landing Zone

### Seguridad

```text
- Detectar conexiones inesperadas
- Detectar escaneos de puertos
- Detectar exfiltración de datos
- Detectar movimiento lateral
```

### Operación

```text
- Troubleshooting de conectividad
- Validación de reglas NSG
- Validación de rutas
- Análisis de tráfico
```

### Compliance

```text
- Evidencia de comunicaciones
- Investigación forense
- Alimentación del SIEM
```

## Recomendación para una Azure Landing Zone

```text
Hub & Spoke
│
├─ Virtual Network Flow Logs
│
├─ Storage Account ZRS
│
├─ Retención 90 días
│
└─ SIEM Corporativo
```

No se recomienda desplegar nuevos NSG Flow Logs.

### Dirección estratégica de Microsoft

```text
NSG Flow Logs  → legado
Virtual Network Flow Logs → futuro
```

Para cubrir el requisito del SIEM de recibir información de tráfico de red, la solución moderna equivalente es Virtual Network Flow Logs.

# Integración con SIEM
````
VNet Flow Logs → Storage Account → SIEM corporativo
````

````
Cada región Azure
 ├─ Network Watcher regional
 ├─ Storage Account regional para VNet Flow Logs
 └─ Export / integración hacia SIEM corporativo
````

Storage Account = ZRS 

## Costs
````
Coste total =
  VNet Flow Logs collected GB
+ Storage Account capacity
+ Storage transactions
+ Data egress si el SIEM está fuera de Azure
+ Event Hub si el SIEM lo requiere
+ Log Analytics / Traffic Analytics si lo activas
````

Virtual Network Flow Logs por GB recogido, con una capa gratuita de 5 GB/mes por suscripción

# ¿Cómo se hace en una Landing Zone?

No se suele hacer manualmente.

Se implementa una Azure Policy:

````
If
   Resource Type = Microsoft.Network/virtualNetworks

Then
   Deploy VNet Flow Logs
````

De esta forma:

````
Nueva VNet creada
       │
       ▼
Azure Policy detecta la VNet
       │
       ▼
Activa automáticamente VNet Flow Logs
       │
       ▼
Envía logs al Storage Account corporativo
````

Lo que yo haría en vuestro ALZ

````
Management Subscription
│
├─ Network Watcher (por región)
├─ Storage Account Flow Logs
├─ Azure Policy DeployIfNotExists
└─ VNet Flow Logs
````

Y la política garantizaría:

````
Todas las VNets productivas
        ↓
Tienen VNet Flow Logs habilitado
````


sin que los equipos de aplicaciones tengan que acordarse de activarlo.

Ese enfoque es el más alineado con una Azure Landing Zone gobernada.

### Connectivity Subscription
Porque todo el ciclo pertenece al dominio de red.
````
Connectivity Subscription
│
├─ Network Watcher
├─ Storage Account Flow Logs
├─ Event Hub (si aplica)
└─ Azure Policies Flow Logs
````


## Connectivity Subscription

### Germany West Central

st-netlogs-prod-gwc-001

```text
st-netlogs-prod-gwc-001
├── VNet Flow Logs
├── Azure Firewall
├── VPN Gateway
└── ExpressRoute Gateway
```

### Sweden Central

st-netlogs-prod-swc-001

```text
st-netlogs-prod-swc-001
├── VNet Flow Logs
├── Azure Firewall
├── VPN Gateway
└── ExpressRoute Gateway
```

---

## Management Subscription

### Germany West Central

st-monitoring-prod-gwc-001

```text
st-monitoring-prod-gwc-001
├── Activity Logs
├── Defender exports
├── Azure Monitor exports
└── Auditoría
```

### Sweden Central

st-monitoring-prod-swc-001

```text
st-monitoring-prod-swc-001
├── Activity Logs
├── Defender exports
├── Azure Monitor exports
└── Auditoría
```
