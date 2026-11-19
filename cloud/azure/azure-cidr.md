[Azure](azure.md)

# 📑 Índice

- [📘 CIDR (Classless Inter-Domain Routing)](#-cidr-classless-inter-domain-routing)
- [📊 Tabla resumen CIDR](#-tabla-resumen-cidr)
- [🧠 Fórmula clave](#-fórmula-clave)
- [🔎 Ejemplo práctico](#-ejemplo-práctico)
- [⚠️ Las 5 IPs reservadas por Azure](#️-las-5-ips-reservadas-por-azure)
- [🌐 GatewaySubnet](#-gatewaysubnet)
- [🎯 Puntos importantes de examen (AZ-104 / AZ-305)](#-puntos-importantes-de-examen-az-104--az-305)
- [📌 Regla rápida mental](#-regla-rápida-mental)

# 📘 CIDR (Classless Inter-Domain Routing)

**CIDR = Classless Inter-Domain Routing**

Permite:
- Enrutamiento entre dominios sin clases
- División flexible del espacio IP
- No usar las antiguas clases A, B, C

Clases antiguas (referencia histórica):
- Clase A → /8  
- Clase B → /16  
- Clase C → /24  

Con CIDR puedes usar cualquier prefijo `/n`.

---

# 📊 Tabla resumen CIDR
| CIDR | 32 - CIDR | IPs Totales | IPs Usables en Azure (-5) | Máscara binaria (para 10.X.X.X) |
|------|-----------|------------|---------------------------|----------------------------------|
| /29  | 3  | 2³ = 8     | 3    | 11111111.11111111.11111111.11111000 |
| /28  | 4  | 2⁴ = 16    | 11   | 11111111.11111111.11111111.11110000 |
| /27  | 5  | 2⁵ = 32    | 27   | 11111111.11111111.11111111.11100000 |
| /26  | 6  | 2⁶ = 64    | 59   | 11111111.11111111.11111111.11000000 |
| /25  | 7  | 2⁷ = 128   | 123  | 11111111.11111111.11111111.10000000 |
| /24  | 8  | 2⁸ = 256   | 251  | 11111111.11111111.11111111.00000000 |
| /23  | 9  | 2⁹ = 512   | 507  | 11111111.11111111.11111110.00000000 |
| /22  | 10 | 2¹⁰ = 1024 | 1019 | 11111111.11111111.11111100.00000000 |


---

## 🧠 Fórmula clave

````
IPs totales: 2^(32 - CIDR)
IPs utilizables en Azure: 2^(32 - CIDR) - 5
````
### 🔎 Ejemplo práctico

````
## Red
10.1.0.0/24

## Cálculo

IPs totales:
2^(32 - 24) = 2^8 = 256

IPs reservadas por Azure: 5

IPs disponibles:
256 - 5 = 251

## Rango completo
10.1.0.0 → 10.1.0.255

## Direcciones especiales en Azure

| Dirección     | Uso                          |
|--------------|-----------------------------|
| 10.1.0.0     | Dirección de red             |
| 10.1.0.1     | Gateway interno Azure        |
| 10.1.0.2     | Azure DNS                    |
| 10.1.0.3     | Reservada uso interno        |
| 10.1.0.255   | Broadcast                    |

## Resumen final

IPs totales: 256  
IPs utilizables en Azure: 251  

````


# ⚠️ Las 5 IPs reservadas por Azure

Azure reserva siempre:

1. Dirección de red  
2. Gateway (.1)  
3. Azure DNS (.2)  
4. Uso interno (.3)  
5. Broadcast  

Esto aplica a todas las subnets en Azure.

---

# 🌐 GatewaySubnet

Subred especial obligatoria para desplegar:

- VPN Gateway
- ExpressRoute Gateway

---

## 🏗 ¿Quién crea la GatewaySubnet?

La **GatewaySubnet la crea el usuario** dentro de la VNet.

Debe:

- Llamarse exactamente:
  GatewaySubnet
- Tener tamaño suficiente:
  - Mínimo recomendado: `/27`
  - Mejor práctica: `/26` o mayor
- No contener otros recursos.

Azure **NO la crea automáticamente**.
Si intentas crear un VPN Gateway sin ella, el portal te pedirá crearla.

---

## ⚙️ ¿Qué ocurre cuando creas un Virtual Network Gateway?

Cuando despliegas un:

- VPN Gateway
- ExpressRoute Gateway

Azure:

1. Usa la subnet llamada `GatewaySubnet`
2. Despliega automáticamente infraestructura administrada por Azure
3. Esa infraestructura son **instancias gestionadas (máquinas virtuales internas de Azure)**

⚠️ Estas VMs:

- No son visibles para el usuario
- No se pueden administrar
- No se pueden acceder por RDP/SSH
- No aparecen en tu listado de VMs

Son parte del **Azure SDN fabric**.

---

## 🌍 ¿Qué hace realmente el Gateway?

Actúa como:

- Punto de conexión entre tu VNet y:
  - Internet (VPN)
  - On-premises
  - Otra VNet
  - ExpressRoute

Flujo simplificado:

VM → 10.X.X.1 (router virtual Azure) → Virtual Network Gateway → Exterior

---

## 🚫 Restricciones importantes

- No se deben desplegar VMs en GatewaySubnet
- No se deben asociar NSG (no recomendado)
- No se deben crear UDR personalizadas ahí
- No usar subnets pequeñas (puede impedir escalado futuro)

---

## 🎯 Punto típico de examen

Pregunta clásica:

> ¿Quién crea la GatewaySubnet?

Respuesta correcta:
El usuario debe crearla manualmente con ese nombre exacto.

Pregunta trampa frecuente:

> ¿Son VMs?

Respuesta:
Sí, internamente Azure usa instancias (VMs administradas),  
pero son completamente gestionadas por Microsoft y no visibles.

---

# 🎯 Puntos importantes de examen (AZ-104 / AZ-305)

- Azure descuenta 5 IPs por subnet.
- No se puede usar una IP individual como subnet (ej: 10.1.0.1/27 ❌).
- Para 500 VMs necesitas al menos:
  - 500 + 5 = 505
  - `/23` → 512 IPs → 507 usables ✅
- Las VNets no pueden solaparse si van a conectarse por VPN o Peering.

# 📌 Regla rápida mental

| Necesitas aprox | Usa |
|----------------|------|
| 250 VMs        | /24 |
| 500 VMs        | /23 |
| 1000 VMs       | /22 |
