[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Route Tables y Next Hop Types (AZ-104)

## Índice

- [Azure Route Tables y Next Hop Types (AZ-104)](#azure-route-tables-y-next-hop-types-az-104)
- [Escenario](#escenario)
- [Qué quiere el ejercicio](#qué-quiere-el-ejercicio)
- [Respuesta correcta](#respuesta-correcta)
- [Qué es una Route Table](#qué-es-una-route-table)
- [Qué es una Route](#qué-es-una-route)
- [Qué es Next Hop](#qué-es-next-hop)
- [Tipos de Next Hop](#tipos-de-next-hop)
- [Virtual Appliance](#virtual-appliance)
- [Qué es una NVA](#qué-es-una-nva)
- [Por qué Virtual Appliance es correcto](#por-qué-virtual-appliance-es-correcto)
- [Qué ocurre realmente](#qué-ocurre-realmente)
- [Ejemplo conceptual](#ejemplo-conceptual)
- [Por qué las otras respuestas son incorrectas](#por-qué-las-otras-respuestas-son-incorrectas)
- [Internet](#internet)
- [Virtual Network Gateway](#virtual-network-gateway)
- [Virtual Network](#virtual-network)
- [Concepto importante examen](#concepto-importante-examen)
- [Trampa típica AZ-104](#trampa-típica-az-104)
- [Tabla resumen Next Hop Types](#tabla-resumen-next-hop-types)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---
## Conceptos

| Concepto | Descripción |
|---|---|
| Route Table | Conjunto de reglas de routing que define por dónde debe viajar el tráfico en Azure |
| Route | Regla individual dentro de una Route Table que define destino y siguiente salto |
| Next Hop | Siguiente dispositivo/recurso al que Azure enviará el tráfico |
| Virtual Appliance | Máquina virtual especializada en networking/security (firewall, router, NVA) |
| NVA (Network Virtual Appliance) | Firewall/router virtual desplegado como VM en Azure |
| UDR (User Defined Route) | Ruta personalizada creada manualmente por el usuario |


## Escenario

Tenemos:

| Recurso | Tipo |
|---|---|
| RT1 | Route Table |

Debemos:

```text
crear una ruta
```

que necesita:

```text
especificar una IP para el next hop
```

---

## Qué quiere el ejercicio

Microsoft pregunta:

```text
¿Qué tipo de Next Hop requiere una IP address?
```

---

## Respuesta correcta

✅

```text
Virtual appliance
```

---

## Qué es una Route Table

Una Route Table contiene reglas de routing.

---

## Para qué sirve

Define:

```text
por dónde debe viajar el tráfico
```

---

## Qué es una Route

Una route define:

| Elemento | Significado |
|---|---|
| Destination | Red destino |
| Next Hop | Siguiente salto |

---

## Qué es Next Hop

El:

```text
Next Hop
```

es:

```text
el siguiente dispositivo/recurso al que Azure enviará el tráfico
```

---

## Tipos de Next Hop

| Tipo | Uso |
|---|---|
| Internet | Salida Internet |
| Virtual Network | Dentro VNet |
| Virtual Network Gateway | VPN/ExpressRoute |
| Virtual Appliance | NVA / Firewall |

---

## Virtual Appliance

Es el único tipo que requiere:

✅ una IP address manual  

---

## Qué es una NVA

NVA significa:

```text
Network Virtual Appliance
```

Es normalmente:

- firewall virtual
- router virtual
- appliance seguridad

ejecutándose como VM.

---

## Ejemplos típicos

| Vendor | Producto |
|---|---|
| Fortinet | FortiGate |
| Palo Alto | VM-Series |
| Cisco | CSR |

---

## Por qué Virtual Appliance es correcto

Porque Azure necesita saber:

```text
la IP privada de la NVA
```

---

## Qué ocurre realmente

La route queda algo así:

| Campo | Valor |
|---|---|
| Destination | 10.2.0.0/16 |
| Next Hop Type | Virtual Appliance |
| Next Hop IP | 10.1.0.4 |

---

## Ejemplo conceptual

```text
Subnet
    ↓
UDR
    ↓
10.1.0.4 (NVA)
    ↓
Firewall inspection
```

---

## Por qué las otras respuestas son incorrectas

## Internet

Azure ya conoce automáticamente:

```text
Internet
```

NO necesitas IP manual.

---

## Virtual Network Gateway

Se usa para:

- VPN Gateway
- ExpressRoute

Azure ya conoce el gateway.

NO necesitas IP manual.

---

## Virtual Network

Se usa para:

```text
routing interno VNet
```

NO necesita IP manual.

---

## Concepto importante examen

Solo:

```text
Virtual Appliance
```

requiere:

```text
Next Hop IP Address
```

---

## Trampa típica AZ-104

Muchos candidatos piensan:

```text
Virtual Network Gateway requiere IP manual
```

❌ Incorrecto.

Azure ya conoce el gateway automáticamente.

---

## Otra trampa típica

Confundir:

```text
Virtual Appliance
```

con:

```text
Virtual Network Gateway
```

---

## Diferencia rápida

| Concepto | Función |
|---|---|
| Virtual Appliance | Firewall/router VM |
| Virtual Network Gateway | VPN/ExpressRoute gateway |

---

## Tabla resumen Next Hop Types

| Next Hop Type | Requiere IP manual | Uso |
|---|---|---|
| Internet | ❌ | Internet |
| Virtual Network | ❌ | Routing interno |
| Virtual Network Gateway | ❌ | VPN/ER |
| Virtual Appliance | ✅ | NVA |

---

## Reglas rápidas AZ-104

```text
Virtual Appliance routes require a next hop IP address.
```

```text
NVAs are commonly used with User Defined Routes.
```

```text
Virtual Network Gateway is used for VPN and ExpressRoute connectivity.
```

---

## Frases clave AZ-104

```text
A virtual appliance is typically a firewall or routing VM.
```

```text
User Defined Routes can redirect traffic to a virtual appliance.
```

```text
Only the Virtual Appliance next hop type requires specifying an IP address.
```
