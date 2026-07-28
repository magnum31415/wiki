[AZ104-INDEX](./readme.md)

# 18 - VPN y ExpressRoute (AZ-104)

> Este documento resume la teoría de **VPN Gateway** y **ExpressRoute** más preguntada en el examen **AZ-104**.

---

# Índice

- Virtual Network Gateway
- GatewaySubnet
- VPN Gateway
- VPN Types
- Site-to-Site VPN
- Point-to-Site VPN
- VNet-to-VNet VPN
- ExpressRoute
- Gateway Transit
- BGP
- Buenas prácticas
- Preguntas trampa

---

# 1. Virtual Network Gateway

El **Virtual Network Gateway** es el recurso que permite conectar una Virtual Network con:

- Otra VNet.
- Redes On-Premises.
- Usuarios remotos.
- ExpressRoute.

No debe confundirse con una **Application Gateway** o un **Load Balancer**.

---

# 2. GatewaySubnet

Todo Virtual Network Gateway debe desplegarse en una Subnet llamada exactamente:

```
GatewaySubnet
```

El nombre es obligatorio.

No puede utilizarse otro nombre.

---

# 3. Tamaño de GatewaySubnet

Microsoft recomienda un tamaño mínimo de:

```
/27
```

Aunque para entornos de producción suele recomendarse:

```
/24
```

para permitir futuras ampliaciones.

---

# 4. VPN Gateway

Un **VPN Gateway** crea un túnel IPsec/IKE cifrado entre Azure y otra red.

Puede utilizarse para:

- Site-to-Site.
- Point-to-Site.
- VNet-to-VNet.

---

# 5. Site-to-Site (S2S)

Permite conectar una red **On-Premises** con una Virtual Network mediante un túnel permanente.

Requiere:

- VPN Gateway en Azure.
- Dispositivo VPN compatible en On-Premises.
- Dirección IP pública en el dispositivo local.

![azure-vpn-site-to-site.png](wiki/img/azure/azure-vpn-site-to-site.png)

---

# 6. Point-to-Site (P2S)

Permite que un usuario individual se conecte desde su equipo a una Virtual Network.

No requiere un dispositivo VPN en la empresa.

Es ideal para:

- Teletrabajo.
- Administradores.
- Acceso remoto.

---

# 7. VNet-to-VNet

Permite conectar dos Virtual Networks mediante VPN.

Cada VNet necesita su propio:

- Virtual Network Gateway.

No basta únicamente con crear un VNet Peering.

---

# 8. VNet Peering vs VNet-to-VNet

| VNet Peering | VNet-to-VNet |
|---------------|--------------|
| Backbone Microsoft | VPN cifrada |
| Mayor rendimiento | Más compatible |
| No necesita Gateway | Requiere Gateway |

Si ambas VNets están en Azure, Microsoft recomienda utilizar **VNet Peering**.

---

# 9. Gateway Transit

**Gateway Transit** permite que varias VNets compartan un único VPN Gateway.

Configuración:

Hub

↓

Use this Virtual Network Gateway

Spoke

↓

Use Remote Gateway

Muy utilizado en arquitecturas **Hub & Spoke**.

---

# 10. ExpressRoute

**ExpressRoute** proporciona una conexión privada entre Azure y las instalaciones del cliente.

No utiliza Internet.

Ventajas:

- Mayor ancho de banda.
- Menor latencia.
- Mayor disponibilidad.
- Mayor seguridad.

---

# 11. VPN vs ExpressRoute

| VPN | ExpressRoute |
|------|--------------|
| Internet | Red privada |
| Menor coste | Mayor coste |
| IPsec/IKE | Circuito dedicado |
| Menor rendimiento | Mayor rendimiento |

---

# 12. Border Gateway Protocol (BGP)

**BGP** permite intercambiar rutas automáticamente entre Azure y la red local.

Ventajas:

- Actualización automática de rutas.
- Alta disponibilidad.
- Escalabilidad.

Es compatible tanto con:

- VPN Gateway.
- ExpressRoute.

---

# 13. Local Network Gateway

El **Local Network Gateway** representa la red On-Premises dentro de Azure.

Define:

- Espacios de direcciones.
- Dirección IP pública del dispositivo VPN.

No es un dispositivo físico.

---

# 14. VPN Gateway SKU

Los VPN Gateway disponen de distintos SKUs.

Los más habituales son:

- VpnGw1
- VpnGw2
- VpnGw3
- VpnGw4
- VpnGw5

A mayor SKU:

- Más rendimiento.
- Más conexiones.
- Mayor coste.

---

# 15. Buenas prácticas

Microsoft recomienda:

- Utilizar **VNet Peering** cuando ambas redes estén en Azure.
- Utilizar **Gateway Transit** en arquitecturas Hub & Spoke.
- Utilizar **ExpressRoute** cuando se necesite máxima disponibilidad y rendimiento.
- Habilitar **BGP** cuando sea posible.

---

# Preguntas trampa del AZ-104

✅ Todo VPN Gateway requiere una Subnet llamada **GatewaySubnet**.

✅ El tamaño recomendado de **GatewaySubnet** es **/27** como mínimo.

✅ **VNet Peering** y **VNet-to-VNet** son tecnologías diferentes.

✅ Una conexión **VNet-to-VNet** requiere un **VPN Gateway en ambas VNets**.

✅ Sin **Gateway Transit**, una VNet no puede utilizar el Gateway de otra.

✅ **ExpressRoute** no utiliza Internet.

✅ **Local Network Gateway** representa la red local; no es un dispositivo físico.

✅ **BGP** permite intercambiar rutas automáticamente entre Azure y la red On-Premises.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Virtual Network Gateway | Conectividad VPN/ExpressRoute |
| GatewaySubnet | Nombre obligatorio |
| Tamaño recomendado | /27 mínimo (/24 recomendado) |
| Site-to-Site | Azure ↔ On-Premises |
| Point-to-Site | Usuario ↔ Azure |
| VNet-to-VNet | VPN entre VNets |
| VNet Peering | Backbone Microsoft |
| Gateway Transit | Compartir VPN Gateway |
| ExpressRoute | Conexión privada |
| BGP | Intercambio automático de rutas |
| Local Network Gateway | Representa la red On-Premises |
| VpnGw1-5 | SKUs del VPN Gateway |
