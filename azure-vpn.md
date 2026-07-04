[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Point-to-Site (P2S) VPN – Secuencia de configuración](#point-to-site-p2s-vpn--secuencia-de-configuración)

---

# Point-to-Site (P2S) VPN – Secuencia de configuración

La secuencia correcta es:

```text
3 → 6 → 4

Crear GatewaySubnet
        ↓
Crear VPN Gateway
        ↓
Configurar IP Address Pool
```

La clave es entender que en una VPN **Point-to-Site (P2S)** hay tres componentes principales.

---

## 1. Crear una subnet adicional: `GatewaySubnet`

Primero necesitas una subnet especial dentro de `VNet1`:

```text
VNet1
│
├── Subnet-VMs
│      ├── VM1
│      └── VM2
│
└── GatewaySubnet
```

Esta subnet debe llamarse exactamente:

```text
GatewaySubnet
```

Ahí Azure despliega las instancias del **VPN Gateway**.

Por eso la primera acción es:

```text
3. Create an additional subnet within VNet1
```

En realidad, la pregunta debería ser más precisa y decir:

```text
Create a GatewaySubnet
```

---

## 2. Desplegar el VPN Gateway

Una vez existe la `GatewaySubnet`, puedes crear el **VPN Gateway**:

```text
Internet
    │
    ▼
VPN Gateway
    │
    ▼
GatewaySubnet
    │
    ▼
VNet1
```

No puedes desplegar el VPN Gateway antes de tener la `GatewaySubnet`.

Por eso:

```text
GatewaySubnet
      ↓
VPN Gateway
```

La segunda acción es:

```text
6. Deploy a VPN gateway for the virtual network
```

---

## 3. Añadir el IP Address Pool

Ahora debes definir qué direcciones IP recibirán los ordenadores de los usuarios cuando se conecten por VPN.

Por ejemplo:

```text
P2S Address Pool

172.16.100.0/24
```

Cuando los usuarios se conectan:

```text
Usuario 1 → 172.16.100.1
Usuario 2 → 172.16.100.2
Usuario 3 → 172.16.100.3
```

Estas IPs son asignadas a los **clientes VPN**, no a las VMs.

Por eso la tercera acción es:

```text
4. Add an IP address pool
```

---

## Arquitectura completa

```text
Casa del usuario
│
│  Laptop
│  IP VPN: 172.16.100.5
│
│  Internet
│
▼
VPN Gateway
│
▼
GatewaySubnet
│
▼
VNet1
│
└── Subnet-VMs
       │
       ├── VM1 → 10.0.1.4
       └── VM2 → 10.0.1.5
```

El usuario puede conectarse mediante RDP:

```text
Laptop
   │
   │ P2S VPN
   ▼
VPN Gateway
   │
   ▼
VM1 Private IP
10.0.1.4:3389
```

La VM **no necesita Public IP**.

---

## ¿Por qué `1 → 6 → 4` es incorrecto?

La secuencia elegida era:

```text
1. Local Network Gateway
       ↓
6. VPN Gateway
       ↓
4. IP Address Pool
```

El error está en el **Local Network Gateway**.

Este recurso representa una **red remota**, normalmente una oficina o datacenter:

```text
Site-to-Site VPN

Azure VNet
    │
VPN Gateway
    │
Internet
    │
Router / Firewall empresa
    │
Red On-Premises
```

En este caso necesitas:

```text
VPN Gateway
+
Local Network Gateway
```

Pero en una conexión **Point-to-Site**:

```text
Usuario individual
      │
      │ VPN
      ▼
Azure VPN Gateway
```

No hay una red on-premises que representar.

---

## Regla mental AZ-104

```text
Point-to-Site (P2S)
Usuario → Azure
│
├── GatewaySubnet
├── VPN Gateway
└── Client Address Pool


Site-to-Site (S2S)
Red empresa → Azure
│
├── GatewaySubnet
├── VPN Gateway
└── Local Network Gateway
```

---

## Respuesta correcta

```text
P2S → 3 → 6 → 4

GatewaySubnet
      ↓
VPN Gateway
      ↓
Client IP Address Pool
```

> **Local Network Gateway no significa "gateway local de Azure". Representa la red remota/on-premises y su dispositivo VPN en una conexión Site-to-Site.**

