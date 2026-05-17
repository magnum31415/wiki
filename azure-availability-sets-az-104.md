[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

- [Azure Compute High Availability Concepts (AZ-104)](#azure-compute-high-availability-concepts-az-104)
- [Azure Availability Sets (AZ-104)](#azure-availability-sets-az-104)

---

# Azure Compute High Availability Concepts (AZ-104)

| Concepto | Qué es | Objetivo principal | Qué hace | Qué NO hace | Ejemplo típico |
|---|---|---|---|---|---|
| Proximity Placement Group (PPG) | Agrupación lógica para colocar recursos físicamente cerca | Reducir latencia | Intenta desplegar VMs en el mismo datacenter/rack cercano | NO da alta disponibilidad | SAP cluster con baja latencia |
| Availability Set | Agrupación lógica de VMs distribuidas entre Fault Domains y Update Domains | Alta disponibilidad local | Separa VMs entre racks y mantenimientos | NO protege una región completa | App web con 2 VMs redundantes |
| Managed Availability Set | Availability Set usando Managed Disks | HA moderna simplificada | Azure administra automáticamente los discos | NO mejora latencia | Availability Set moderno con Managed Disks |
| Fault Domain (FD) | Grupo de hardware físico independiente | Evitar fallo hardware único | Distribuye VMs entre racks/alimentación distintos | NO protege contra mantenimiento | VM1 en RackA, VM2 en RackB |

---

# Relación entre conceptos

```text
Availability Set
 ├── Fault Domain 0
 │     └── VM1
 │
 ├── Fault Domain 1
 │     └── VM2
 │
 └── Fault Domain 2
       └── VM3
```

---

# Availability Set + PPG

```text
Proximity Placement Group
 └── Availability Set
       ├── VM1
       ├── VM2
       └── VM3
```

↓

Azure intenta:

- mantener baja latencia
- pero separando VMs entre Fault Domains

---

# Ejemplos reales

| Escenario | Solución |
|---|---|
| SAP HANA cluster | PPG + Availability Set |
| Aplicación web HA | Availability Set |
| Baja latencia entre VMs | PPG |
| Protección fallo rack | Fault Domains |
| Diseño moderno Azure | Managed Availability Set |

---

# Diferencia importante

| Concepto | Junta VMs | Separa VMs |
|---|---|---|
| PPG | ✅ | ❌ |
| Availability Set | ❌ | ✅ |

---

# Concepto clave examen

```text
PPG reduce latency.
```

```text
Availability Set improves availability.
```

```text
Fault Domains separate hardware failure boundaries.
```

```text
Managed Availability Sets use managed disks.
```

---


# Azure Availability Sets (AZ-104)

# Qué es un Availability Set

Un:

```text
Availability Set
```

permite distribuir VMs entre:

- Fault Domains
- Update Domains

para reducir:

```text
single points of failure
```

---

# Objetivo principal

Evitar que:

- mantenimiento Azure
- fallos hardware
- reinicios host

afecten a TODAS las VMs a la vez.

---

# Conceptos clave

| Concepto | Explicación |
|---|---|
| Fault Domain (FD) | Rack/hardware distinto |
| Update Domain (UD) | Grupo reiniciado conjuntamente durante mantenimiento |

---

# Fault Domains

Protegen frente a:

```text
fallos físicos hardware
```

Ejemplo:

- fallo rack
- fallo alimentación
- fallo switch físico

---

# Visualmente

```text
Rack 1 → VM1
Rack 2 → VM2
Rack 3 → VM3
```

↓

si cae un rack:

```text
las otras VMs sobreviven
```

---

# Update Domains

Protegen frente a:

```text
mantenimiento planificado Azure
```

Azure reinicia:

```text
un Update Domain cada vez
```

NO todos simultáneamente.

---

# Ejemplo

| VM | Update Domain |
|---|---|
| VM1 | UD0 |
| VM2 | UD1 |
| VM3 | UD2 |

---

# Concepto importante AZ-104

Availability Set:

```text
NO protege contra caída completa de región
```

↓

solo dentro del mismo datacenter.

---

# Availability Set vs Availability Zone

| Característica | Availability Set | Availability Zone |
|---|---|---|
| Protección rack/hardware | ✅ | ✅ |
| Protección datacenter completo | ❌ | ✅ |
| Región única | ✅ | ✅ |
| Distintos datacenters físicos | ❌ | ✅ |
| Baja latencia | Alta | Menor |
| Coste adicional | ❌ | Puede |

---

# Availability Set usa

```text
Managed Disks
```

modernamente.

---

# La pregunta

Tienes:

```text
VM1, VM2, VM3
```

en:

```text
AVSet1
```

---

# Problema

Quieres cambiar:

```text
el tamaño VM1
```

pero Azure dice:

```text
target size unavailable
```

---

# Qué significa realmente

La VM está:

```text
asignada a hardware concreto
```

↓

y en ese cluster/hardware:

```text
NO hay capacidad para ese tamaño
```

---

# Solución correcta

```text
Deallocate VM1
```

---

# Por qué funciona

Cuando haces:

```text
Stop
```

NO siempre libera hardware.

---

# Diferencia IMPORTANTÍSIMA

| Acción | Libera hardware Azure |
|---|---|
| Power Off dentro SO | ❌ |
| Stop Azure Portal | ✅ parcialmente |
| Deallocate | ✅ Sí |

---

# Qué hace Deallocate

Azure:

- libera compute host
- libera placement
- libera capacidad asignada

↓

y puede mover la VM a otro hardware compatible.

---

# Entonces

Después de:

```text
Deallocate
```

↓

Azure puede:

```text
reasignar VM1 a hardware con el nuevo tamaño
```

---

# Concepto MUY importante examen

Resize VM muchas veces requiere:

```text
deallocate
```

---

# Trampa típica AZ-104

Muchos creen:

```text
Power Off = Deallocate
```

❌ Incorrecto.

---

# Diferencia visual

## Power Off dentro SO

```text
VM apagada
hardware reservado
```

---

## Deallocate

```text
VM apagada
hardware liberado
```

---

# Por qué las otras respuestas son incorrectas

---

# Proximity Placement Group

Sirve para:

```text
baja latencia
```

NO para:

```text
resolver capacity resize
```

---

# Managed Availability Set

No cambia:

```text
capacidad hardware disponible
```

---

# Apagar VM2 y VM3

No afecta:

```text
al host/capacidad asignada VM1
```

---

# Conceptos importantes AZ-104

| Concepto | Importancia |
|---|---|
| Availability Set | Muy alta |
| Fault Domains | Muy alta |
| Update Domains | Muy alta |
| Resize VM | Alta |
| Deallocate | Muy alta |
| Managed disks | Alta |

---

# Tabla rápida importante

| Acción | VM running | Hardware reservado |
|---|---|---|
| Shutdown SO | ❌ | ✅ |
| Stop Azure | ❌ | Puede |
| Deallocate | ❌ | ❌ |

---

# Cuándo usar Availability Sets

| Escenario | Recomendado |
|---|---|
| Alta disponibilidad básica | ✅ |
| Aplicaciones legacy | ✅ |
| Múltiples VMs mismo datacenter | ✅ |
| Protección regional | ❌ |

---

# Cuándo usar Availability Zones

| Escenario | Recomendado |
|---|---|
| Protección datacenter completo | ✅ |
| Alta resiliencia enterprise | ✅ |
| Disaster tolerance | ✅ |

---

# Límites importantes

| Característica | Availability Set |
|---|---|
| Fault Domains típicos | 2 o 3 |
| Update Domains | Hasta 20 |

---

# Reglas rápidas examen

```text
Availability Sets distribute VMs across fault and update domains.
```

```text
Deallocate releases the VM hardware allocation.
```

```text
Stopping a VM is not always the same as deallocating it.
```

```text
Availability Sets do not protect against regional failures.
```
