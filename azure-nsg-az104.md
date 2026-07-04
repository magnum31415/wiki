[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


# Indice

- [Azure Network Security Groups (NSG) - AZ-104)](#azure-network-security-groups-nsg---az-104)

  
---

# Azure Network Security Groups (NSG) - AZ-104

# Qué es un NSG

Un:

```text
Network Security Group (NSG)
```

es un firewall Layer 4 que permite:

- Allow
- Deny

tráfico:

- inbound
- outbound

para:

- subnets
- network interfaces (NICs)


**Una regla dentro de un NSG no hace absolutamente nada si el NSG no está asociado.**

````
Crear NSG
    ↓
Crear reglas
    ↓
Asociar NSG (Subnet y/o Network Interface)
    ↓
Las reglas empiezan a aplicarse
````
---

# Qué filtra un NSG

| Elemento | Ejemplo |
|---|---|
| Source IP | Internet |
| Destination IP | VM |
| Port | 3389 |
| Protocol | TCP/UDP |
| Direction | Inbound/Outbound |

---

# Dónde puede asociarse

| Asociación | Explicación |
|---|---|
| Subnet | Afecta todas las VMs subnet |
| NIC | Afecta solo una VM/NIC |

---

# MUY IMPORTANTE EXAMEN

Un NSG puede aplicarse:

```text
simultáneamente
```

a:

- subnet
- NIC

---

# Orden evaluación NSG

## Inbound traffic

```text
Internet
   ↓
Subnet NSG
   ↓
NIC NSG
   ↓
VM
```

---

## Outbound traffic

```text
VM
   ↓
NIC NSG
   ↓
Subnet NSG
   ↓
Internet
```

---

# Regla IMPORTANTÍSIMA

El tráfico debe ser:

```text
permitido en TODOS los niveles
```

---

# Si un NSG bloquea

↓

el tráfico:

```text
se DENIEGA
```

aunque otro lo permita.

---

# Concepto clave examen

```text
Deny wins.
```

---

# Prioridades NSG

MUY importante AZ-104.

---

# Cómo funcionan

| Prioridad | Valor |
|---|---|
| Menor número | MÁS prioridad |
| Mayor número | MENOS prioridad |

---

# Ejemplo

| Priority | Acción |
|---|---|
| 100 | Allow RDP |
| 200 | Deny All |

↓

gana:

```text
Allow RDP
```

porque:

```text
100 < 200
```

---

# Regla IMPORTANTÍSIMA

```text
Azure procesa reglas de menor a mayor prioridad
```

---

# Rango prioridades

```text
100 → 4096
```

---

# Default Rules

Todos los NSG tienen reglas por defecto.

---

# Default inbound rules

| Priority | Nombre | Acción | Qué significa |
|---|---|---|---|
| 65000 | AllowVNetInBound | Allow | Permite tráfico desde la misma VNet |
| 65001 | AllowAzureLoadBalancerInBound | Allow | Permite tráfico desde Azure Load Balancer |
| 65500 | DenyAllInbound | Deny | Bloquea TODO el tráfico inbound restante |

---

# Default outbound rules

| Priority | Nombre | Acción | Qué significa |
|---|---|---|---|
| 65000 | AllowVnetOutbound | Allow | Permite tráfico hacia la misma VNet |
| 65001 | AllowInternetOutbound | Allow | Permite salida hacia Internet |
| 65500 | DenyAllOutbound | Deny | Bloquea resto tráfico outbound |


---

# MUY importante

Por defecto:

```text
Internet → VM
```

↓

está:

```text
DENEGADO
```

porque existe:

```text
DenyAllInbound
```

---

# La pregunta del examen

## VM1

Está en:

```text
Subnet1
```

↓

Subnet1 tiene:

```text
NSG1
```

---

# NSG1 solo tiene:

```text
default rules
```

---

# Entonces

No existe ninguna regla:

```text
Allow TCP 3389
```

desde Internet.

---

# Resultado

El tráfico llega a:

```text
DenyAllInbound (65500)
```

↓

RDP bloqueado.

---

# Por eso la respuesta correcta es

```text
False
```

---

# Aunque la VM tenga Public IP

MUY típica trampa examen.

---

# Importante

```text
Public IP ≠ acceso permitido
```

También necesitas:

```text
NSG allow rule
```

---

# Por qué VM2 sí funcionaría

VM2 tiene:

```text
NSG2 asociado a la NIC
```

con:

| Priority | Puerto | Acción |
|---|---|---|
| 100 | 3389 | Allow |

↓

antes de:

```text
DenyAllInbound
```

↓

RDP permitido.

---

# Reglas opuestas

MUY importante examen.

---

# Ejemplo

| Priority | Acción |
|---|---|
| 100 | Deny 3389 |
| 200 | Allow 3389 |

↓

gana:

```text
Deny
```

porque:

```text
100 tiene más prioridad
```

---

# Otro ejemplo

| Priority | Acción |
|---|---|
| 100 | Allow 3389 |
| 200 | Deny 3389 |

↓

gana:

```text
Allow
```

---

# Concepto clave

```text
FIRST MATCH WINS
```

---

# Azure deja de evaluar

cuando encuentra:

```text
la primera coincidencia
```

---

# Stateful NSG

MUY importante.

---

# NSGs son:

```text
stateful
```

---

# Qué significa

Si permites:

```text
inbound
```

↓

la respuesta outbound:

```text
se permite automáticamente
```

---

# No necesitas

regla explícita retorno.

---

# Service Tags importantes

| Service Tag | Significado |
|---|---|
| Internet | Internet |
| VirtualNetwork | VNet |
| AzureLoadBalancer | Azure LB |
| Storage | Azure Storage |

---

# ASG (Application Security Groups)

Permiten agrupar VMs lógicamente.

---

# Ejemplo

```text
ASG-Web
ASG-DB
```

↓

reglas:

```text
Web → DB : 1433
```

---

# Trampas típicas AZ-104

## Trampa 1

```text
Public IP = acceso permitido
```

❌ Incorrecto.

---

# Trampa 2

```text
NSG subnet override NSG NIC
```

❌ Incorrecto.

Ambos se evalúan.

---

# Trampa 3

```text
Allow gana siempre
```

❌ Incorrecto.

Gana:

```text
menor priority number
```

---

# Trampa 4

```text
NSG son stateless
```

❌ Incorrecto.

Son:

```text
stateful
```

---

# Tabla resumen MUY importante

| Concepto | Regla |
|---|---|
| Menor número prioridad | Más prioridad |
| Primera coincidencia | Gana |
| Public IP | No abre acceso |
| NSG subnet + NIC | Ambos se aplican |
| Stateful | Sí |
| Default inbound internet | Deny |

---

# Reglas rápidas examen

```text
NSGs are stateful packet filtering firewalls.
```

```text
Lower priority number = higher priority.
```

```text
First matching rule is applied.
```

```text
Public IP addresses do not bypass NSG rules.
```

```text
Both subnet and NIC NSGs are evaluated.
```
---

En NSG:

la primera regla que coincide gana

y Azure evalúa:

de menor número → mayor número
En tu ejemplo
Priority	Acción
100	Allow RDP
200	Deny All

Azure empieza por:

100

↓

¿Coincide tráfico RDP?

✅ Sí.

↓

Entonces:

SE DETIENE la evaluación

↓

Resultado:

Allow
La regla 200
ni siquiera se evalúa

porque Azure ya encontró:

la primera coincidencia
Concepto clave NSG
FIRST MATCH WINS
