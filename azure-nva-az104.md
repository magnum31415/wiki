[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# NVA, UDR y Routing entre VNets (AZ-104)

## Índice

- [NVA, UDR y Routing entre VNets (AZ-104)](#nva-udr-y-routing-entre-vnets-az-104)
- [Escenario](#escenario)
- [Qué quiere el ejercicio](#qué-quiere-el-ejercicio)
- [Respuesta correcta](#respuesta-correcta)
- [Qué es una NVA](#qué-es-una-nva)
- [Qué es Virtual Network Peering](#qué-es-virtual-network-peering)
- [Problema importante](#problema-importante)
- [Qué es una Route Table](#qué-es-una-route-table)
- [Qué es una Custom Route](#qué-es-una-custom-route)
- [Qué es una UDR](#qué-es-una-udr)
- [Cómo funciona realmente](#cómo-funciona-realmente)
- [Flujo normal sin UDR](#flujo-normal-sin-udr)
- [Flujo correcto con NVA](#flujo-correcto-con-nva)
- [Qué se configura realmente](#qué-se-configura-realmente)
- [Qué significa Next Hop](#qué-significa-next-hop)
- [Ejemplo conceptual](#ejemplo-conceptual)
- [Por qué las otras respuestas son incorrectas](#por-qué-las-otras-respuestas-son-incorrectas)
- [Local Network Gateway](#local-network-gateway)
- [Service Endpoint](#service-endpoint)
- [IP Address Reservations](#ip-address-reservations)
- [Concepto clave examen](#concepto-clave-examen)
- [Trampa típica AZ-104](#trampa-típica-az-104)
- [Qué quiere evaluar Microsoft](#qué-quiere-evaluar-microsoft)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

## Escenario

Tenemos:

| Recurso | Descripción |
|---|---|
| VNet1 | Virtual Network |
| VNet2 | Virtual Network |
| Peering | Conectadas mediante peering |
| NetVA1 | Network Virtual Appliance |

---

## Qué quiere el ejercicio

Microsoft quiere que:

```text
todo el tráfico entre VNet1 y VNet2
```

pase primero por:

```text
NetVA1
```

para ser inspeccionado.

---

## Respuesta correcta

✅

```text
a route table that has custom routes
```

---

## Qué es una NVA

NVA significa:

```text
Network Virtual Appliance
```

Es una VM especializada en networking y seguridad.

---

## Qué puede hacer una NVA

| Función | Ejemplo |
|---|---|
| Firewall | Filtrar tráfico |
| IDS/IPS | Inspección paquetes |
| Routing | Redirigir tráfico |
| VPN | Conectividad segura |
| NAT | Traducción IPs |

---

## Idea importante

La NVA actúa como:

```text
firewall/router virtual
```

dentro de Azure.

---

## Qué es Virtual Network Peering

El peering conecta VNets directamente.

---

## Importante

Con peering:

✅ tráfico privado rápido  
✅ comunicación directa  

pero normalmente:

❌ el tráfico NO pasa por firewalls automáticamente

---

## Problema importante

Sin configuración adicional:

```text
VNet1
    ↓
directamente
    ↓
VNet2
```

La NVA:

❌ NO inspecciona tráfico.

---

## Qué es una Route Table

Una Route Table contiene reglas routing.

---

## Qué define

Define:

```text
por dónde debe viajar el tráfico
```

---

## Qué es una Custom Route

Es una ruta creada manualmente.

También llamada:

```text
User Defined Route (UDR)
```

---

## Qué es una UDR

UDR significa:

```text
User Defined Route
```

Permite sobrescribir rutas automáticas Azure.

---

## Cómo funciona realmente

La UDR fuerza:

```text
tráfico destino VNet2
    ↓
NetVA1
    ↓
VNet2
```

---

## Flujo normal sin UDR

```text
VNet1
    ↓
Peering
    ↓
VNet2
```

La NVA no participa.

---

## Flujo correcto con NVA

```text
VNet1
    ↓
Route Table (UDR)
    ↓
NetVA1
    ↓
VNet2
```

---

## Qué se configura realmente

En la route table:

| Configuración | Valor |
|---|---|
| Destination | Red VNet2 |
| Next Hop Type | Virtual Appliance |
| Next Hop IP | IP privada NetVA1 |

---

## Qué significa Next Hop

Es:

```text
el siguiente salto/redirección del tráfico
```

---

## Ejemplo conceptual

```text
10.2.0.0/16
    ↓
Next Hop
    ↓
10.1.0.4 (NetVA1)
```

---

## Por qué las otras respuestas son incorrectas

## Local Network Gateway

Se usa para:

✅ VPN híbridas  
✅ On-premises  

NO para:

❌ routing entre VNets peered

---

## Service Endpoint

Se usa para:

✅ acceso privado a servicios Azure

Ejemplo:

- Storage
- SQL
- Key Vault

NO para:

❌ inspección tráfico

---

## IP Address Reservations

Solo reserva IPs.

NO hace:

❌ routing  
❌ inspección  
❌ redirección tráfico  

---

## Concepto clave examen

```text
Para forzar tráfico a pasar por una NVA
→ se usan UDRs / custom routes
```

---

## Trampa típica AZ-104

Muchos candidatos creen:

```text
Peering = tráfico inspeccionado
```

❌ Incorrecto.

El peering conecta VNets:

pero NO fuerza inspección.

---

## Otra trampa típica

Muchos creen:

```text
NVA automáticamente inspecciona tráfico
```

❌ Incorrecto.

Necesitas:

✅ Route Table  
✅ UDR  
✅ Next Hop  

---

## Qué quiere evaluar Microsoft

| Concepto | Importancia |
|---|---|
| UDR | Muy alta |
| NVA | Muy alta |
| Route Tables | Muy alta |
| VNet Peering | Alta |
| Next Hop | Alta |

---

## Tabla resumen examen

| Necesidad | Solución |
|---|---|
| Inspeccionar tráfico | NVA |
| Forzar tráfico hacia NVA | UDR |
| Routing personalizado | Route Table |
| Comunicación privada VNets | Peering |

---

## Reglas rápidas AZ-104

```text
UDRs are used to force traffic through NVAs.
```

```text
Peering alone does not provide traffic inspection.
```

```text
A Network Virtual Appliance acts as a virtual firewall/router.
```

---

## Frases clave AZ-104

```text
Custom routes can redirect traffic to a virtual appliance.
```

```text
User Defined Routes override Azure system routes.
```

```text
The next hop can be configured as a virtual appliance.
```
