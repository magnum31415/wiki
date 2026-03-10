# Hub and Spoke en Azure 

## 1. ¿Qué es Hub and Spoke?

**Hub and Spoke** es un **modelo de arquitectura de red** usado en Azure para organizar y controlar la conectividad entre redes virtuales (**Virtual Networks - VNets**).

La idea es separar:

- **Hub** → red central con servicios compartidos
- **Spokes** → redes donde viven las aplicaciones o workloads

El tráfico entre spokes normalmente pasa por el hub.

````
           Spoke VNet (App1)
                 |
                 |
Spoke VNet ---- HUB ---- Spoke VNet
 (App2)          |        (App3)
                 |
                 |
           Spoke VNet (Data)
````

---

# Hub (red central)

La red **hub** contiene servicios de infraestructura compartidos.

Ejemplos comunes:

- Azure Firewall
- VPN Gateway
- ExpressRoute Gateway
- Azure Bastion
- DNS
- Network Virtual Appliances (NVA)
- Private DNS Zones
- Routing centralizado
- Logging y monitorización

---

# Spokes (redes de workloads)

Cada **spoke** suele representar:

- una aplicación
- un entorno
- un dominio funcional

Ejemplos:

```
vnet-app-prod
vnet-data-platform
vnet-ai
vnet-dev
```

Las spokes normalmente:

- **no se conectan entre ellas directamente**
- se conectan al **hub mediante VNet Peering**

---

# 2. Ventajas del modelo Hub and Spoke

## Seguridad centralizada

Todo el tráfico puede pasar por el hub:

```
Spoke → Hub Firewall → Internet
Spoke → Hub Firewall → Spoke
```

Esto permite:

- inspección de tráfico
- filtrado
- logging
- control de acceso

---

## Reutilización de servicios

Servicios comunes se despliegan una sola vez en el hub:

- Firewall
- Bastion
- VPN
- DNS

Todos los spokes los utilizan.

---

## Aislamiento de workloads

Cada workload tiene su propia red.

Ejemplo:

```
Spoke-ERP
Spoke-AI
Spoke-Data
Spoke-Web
```

Si una red se ve comprometida, el impacto queda limitado.

---

## Escalabilidad

Se pueden añadir nuevos spokes sin rediseñar toda la red.

```
Hub
 ├── Spoke App1
 ├── Spoke App2
 ├── Spoke Data
 ├── Spoke AI
 └── Spoke Dev
```

---

# 3. Componentes típicos del Hub

| Componente | Función |
|---|---|
| Azure Firewall | Control del tráfico |
| VPN Gateway | Conexión con on-prem |
| ExpressRoute | Conexión privada |
| Azure Bastion | Acceso seguro a VMs |
| Private DNS | Resolución de nombres |
| Route Tables | Control de rutas |
| Log Analytics | Monitorización |

---

# 4. Cómo implementarlo en una Azure Landing Zone

En una **Azure Landing Zone (CAF / Enterprise Scale)** normalmente se utiliza este modelo.

## Paso 1 — Crear la VNet del Hub

Ejemplo:

```
vnet-hub-network
10.0.0.0/16
```

Subnets típicas:

```
AzureFirewallSubnet
GatewaySubnet
AzureBastionSubnet
SharedServicesSubnet
```

---

## Paso 2 — Desplegar servicios compartidos

En el hub se despliegan servicios como:

- Azure Firewall
- Bastion
- VPN Gateway
- Private DNS

---

## Paso 3 — Crear VNets Spoke

Ejemplo:

```
vnet-app-prod
10.1.0.0/16

vnet-data
10.2.0.0/16

vnet-ai
10.3.0.0/16
```

Subnets típicas:

```
subnet-web
subnet-app
subnet-db
```

---

## Paso 4 — Crear VNet Peering

Conectar cada spoke al hub:

```
Hub VNet  <---->  Spoke VNet
```

Configuración típica:

```
Allow forwarded traffic = true
Allow gateway transit = true
Use remote gateway = true (en spoke)
```

---

## Paso 5 — Routing centralizado

Se usan **User Defined Routes (UDR)** para forzar tráfico hacia el firewall.

Ejemplo:

```
0.0.0.0/0 → Azure Firewall
```

Esto obliga a que todo el tráfico salga a través del hub.

---

# 5. Cómo saber si tu tenant ya usa Hub and Spoke

## Método 1 — Revisar VNets

```bash
az network vnet list -o table
```

Si aparecen redes como:

```
vnet-hub
vnet-connectivity
vnet-shared-services
vnet-app-prod
vnet-data
vnet-ai
```

probablemente existe un hub.

---

## Método 2 — Revisar VNet Peerings

```bash
az network vnet peering list \
  --resource-group RG-NETWORK \
  --vnet-name vnet-hub \
  -o table
```

Si aparecen conexiones como:

```
vnet-hub  <-> vnet-app
vnet-hub  <-> vnet-data
vnet-hub  <-> vnet-ai
```

es una topología hub-and-spoke.

---

# Resumen

Hub and Spoke significa:

```
Hub = red central con servicios compartidos
Spokes = redes de aplicaciones
```

Ventajas:

- seguridad centralizada
- reutilización de servicios
- aislamiento de workloads
- escalabilidad
