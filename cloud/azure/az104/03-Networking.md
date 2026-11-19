[AZ104-INDEX](./readme.md)

# 03 - Networking (AZ-104)

> Este documento resume los conceptos de **Azure Networking** más preguntados en el examen **AZ-104**.

---

# Índice

- Virtual Network (VNet)
- Address Spaces
- Subnets
- Reserved IPs
- Network Interface (NIC)
- Public IP
- Private IP
- VNet Peering
- Gateway Transit
- User Defined Routes (UDR)
- Route Tables
- Service Chaining
- IP Forwarding
- Preguntas trampa

---

# 1. Virtual Network (VNet)

Una **Virtual Network (VNet)** es una red privada dentro de Azure donde se despliegan recursos como:

- Virtual Machines
- Load Balancers
- Azure Bastion
- Azure Firewall

Las máquinas conectadas a la misma VNet pueden comunicarse mediante **direcciones IP privadas**.

---

# 2. Address Space

Cada VNet debe tener uno o varios **Address Spaces** definidos mediante **CIDR**.

Ejemplo:

```
10.0.0.0/16
```

Una VNet puede tener **varios Address Spaces**.

Ejemplo:

```
10.0.0.0/16

192.168.0.0/24
```

---

# 3. Subnets

Las **Subnets** dividen una Virtual Network en segmentos más pequeños.

Ejemplo:

```
VNet

10.0.0.0/16

│

├── Subnet1
│   10.0.1.0/24
│
├── Subnet2
│   10.0.2.0/24
│
└── AzureBastionSubnet
```

Cada recurso se conecta siempre a una Subnet.

---

# 4. Reserved IP Addresses

Azure reserva automáticamente **5 direcciones IP** en cada Subnet.

Ejemplo:

```
10.0.1.0/24
```

Azure reserva:

```
.0    → Dirección de red (Network Address)
.1    → Gateway predeterminado de Azure
.2    → Servidor DNS de Azure
.3    → Reservada para uso interno de Azure
.255  → Última dirección de la subred (reservada por Azure)
```

Estas direcciones no pueden asignarse a recursos.

---

# 5. Private IP

Todos los recursos de una VNet disponen de una **Private IP**.

Puede configurarse como:

- Dynamic
- Static

Las máquinas virtuales utilizan la Private IP para comunicarse dentro de Azure.

---

# 6. Public IP

Una **Public IP** permite acceder desde Internet.

Puede ser:

- Basic
- Standard

Y configurarse como:

- Static
- Dynamic

Muchos servicios modernos requieren **Standard SKU**.

---

# 7. Network Interface (NIC)

Toda máquina virtual necesita al menos una **Network Interface (NIC)**.

La NIC debe crearse en:

- la misma Subscription
- la misma Región
- una Subnet de la VNet

No puede conectarse a una VNet de otra región.

---

# 8. VNet Peering

El **VNet Peering** conecta dos Virtual Networks mediante la red troncal de Microsoft.

Permite comunicación mediante IP privada.

No utiliza VPN Gateway.

---

## Requisitos

Las VNets:

- no pueden solaparse
- pueden estar en distintas regiones (Global Peering)

---

## CIDR Overlap

Si los Address Spaces se solapan:

```
10.0.0.0/16

10.0.1.0/24
```

↓

No podrá crearse el Peering.

Es una pregunta muy habitual.

---

# 9. Global VNet Peering

Permite conectar VNets situadas en regiones diferentes.

El comportamiento es prácticamente idéntico al Peering regional.

---

# 10. Gateway Transit

Permite que una VNet utilice el **VPN Gateway** de otra.

Configuración:

Hub

↓

Use this Virtual Network Gateway

Spoke

↓

Use Remote Gateway

Sin Gateway Transit **no existe tránsito** entre VNets.

---

# 11. VNet Peering NO es transitivo

Si:

```
A

↓

B

↓

C
```

A está conectada con B.

B está conectada con C.

**A NO puede comunicarse con C**.

Azure **no soporta peering transitivo**.

Muy preguntado en AZ-104.

---

# 12. User Defined Routes (UDR)

Las **User Defined Routes** permiten modificar el enrutamiento por defecto de Azure.

Se almacenan dentro de una:

**Route Table**

---

# 13. Route Tables

Una **Route Table** contiene una o varias rutas personalizadas.

Puede asociarse a una o varias Subnets.

Ejemplo:

```
Destino

0.0.0.0/0

↓

Azure Firewall
```

Todo el tráfico saldrá a través del Firewall.

---

# 14. Next Hop Types

Los principales Next Hop son:

- Virtual Appliance
- Internet
- Virtual Network
- Virtual Network Gateway
- None

Pregunta muy frecuente.

---

# 15. Service Chaining

Una **UDR** puede enviar tráfico hacia una máquina virtual que actúe como:

- Firewall
- Router
- Appliance

Esto recibe el nombre de:

**Service Chaining**

---

# 16. IP Forwarding

Si una máquina virtual debe actuar como Router o Firewall es necesario habilitar:

**IP Forwarding**

En:

- la NIC
- el sistema operativo

---

# 17. Effective Routes

Cada NIC dispone de unas:

**Effective Routes**

Son el resultado de combinar:

- rutas del sistema
- UDR
- BGP

Muy útil para diagnosticar problemas de conectividad.

---

# 18. DNS

Por defecto las VNets utilizan:

**Azure-provided DNS**

También es posible configurar:

- Custom DNS Server

Todas las máquinas de la VNet utilizarán ese servidor DNS.

---

# 19. Azure-provided DNS

Cuando una VNet utiliza Azure DNS:

- resuelve nombres internos
- resuelve Private DNS Zones

Si se configura un **Custom DNS**, Azure deja de utilizar el DNS interno.

---

# 20. Buenas prácticas

Microsoft recomienda:

- Evitar CIDR solapados.
- Utilizar Private IP siempre que sea posible.
- Utilizar VNet Peering antes que VPN cuando ambas VNets están en Azure.
- Centralizar conectividad mediante arquitectura Hub & Spoke.
- Utilizar UDR únicamente cuando sea necesario modificar el enrutamiento.

---

# Preguntas trampa

✅ Una **VNet puede tener varios Address Spaces**.

✅ Las **Subnets no pueden solaparse**.

✅ Azure reserva **5 direcciones IP** por Subnet.

✅ Una **NIC** debe estar en la misma región que la **VNet**.

✅ Un **VNet Peering no es transitivo**.

✅ El **Gateway Transit** debe configurarse explícitamente.

✅ Las VNets con **CIDR solapado** no pueden hacer Peering.

✅ Las **UDR** se almacenan en una **Route Table**.

✅ Para utilizar una VM como Firewall debe habilitarse **IP Forwarding**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| VNet | Red privada de Azure |
| Address Space | Puede haber varios |
| Subnet | Divide la VNet |
| Azure reserva | 5 IPs |
| NIC | Misma región que la VNet |
| VNet Peering | No usa VPN |
| Global Peering | Regiones distintas |
| Gateway Transit | Compartir VPN Gateway |
| Peering | No transitivo |
| UDR | Modifica rutas |
| Route Table | Contiene UDR |
| Service Chaining | VM como Firewall |
| IP Forwarding | Obligatorio para routers |
| Effective Routes | Rutas realmente utilizadas |
| Azure DNS | DNS por defecto |
