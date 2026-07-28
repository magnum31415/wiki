[AZ104-INDEX](./readme.md)

# 14 - Virtual Machines (AZ-104)

> Este documento resume la teoría de **Azure Virtual Machines** más preguntada en el examen **AZ-104**.

---

# Índice

- Azure Virtual Machines
- Availability Sets
- Availability Zones
- Managed Disks
- Tamaños de VM
- Resize
- Redeploy
- Reapply
- VM Extensions
- Azure VM Backup
- VM Scale Sets
- Buenas prácticas
- Preguntas trampa

---

# 1. Azure Virtual Machines

Las **Azure Virtual Machines (VMs)** proporcionan infraestructura como servicio (**IaaS**), permitiendo ejecutar Windows o Linux con control total sobre el sistema operativo.

Cada VM necesita como mínimo:

- Una **Virtual Network**.
- Una **Subnet**.
- Una **Network Interface (NIC)**.
- Un **disco del sistema operativo**.

---

# 2. Availability Sets

Los **Availability Sets** protegen frente a:

- Fallos de hardware.
- Reinicios por mantenimiento planificado.

Azure distribuye automáticamente las VMs entre distintos:

- **Fault Domains**
- **Update Domains**

---

# 3. Availability Zones

Las **Availability Zones** distribuyen las VMs entre centros de datos físicamente independientes dentro de una región.

Protegen frente a la pérdida completa de un datacenter.

No todas las regiones soportan Availability Zones.

---

# 4. Managed Disks

Microsoft recomienda utilizar siempre **Managed Disks**.

Ventajas:

- Azure administra automáticamente el almacenamiento.
- Mayor disponibilidad.
- Integración con Backup y Snapshots.
- Mejor escalabilidad.

---

# 5. Tipos de discos

| Disco | Uso |
|--------|-----|
| Standard HDD | Desarrollo |
| Standard SSD | Producción básica |
| Premium SSD | Alto rendimiento |
| Ultra Disk | Máximo rendimiento |

---

# 6. Cambiar el tamaño de una VM (Resize)

El tamaño de una VM puede modificarse después de su creación.

El nuevo tamaño debe estar disponible en la región y, en algunos casos, en el mismo clúster físico donde se encuentra la VM.

Si no está disponible, será necesario detener (**Stop/Deallocate**) la VM para que Azure pueda moverla a otro clúster.

---

# Operaciones que requieren reinicio o parada

| Operación | ¿Requiere reinicio? | ¿Requiere Stop (Deallocate)? |
|-----------|:-------------------:|:----------------------------:|
| Cambiar el tamaño (Resize) dentro del mismo clúster | ✅ Sí | ❌ No (en algunos casos) |
| Cambiar el tamaño a uno no disponible en el clúster | ✅ Sí | ✅ Sí |
| **Aumentar o disminuir el número de vCPU** | ✅ Sí (Resize) | ✅ Puede ser necesario |
| **Aumentar o disminuir la memoria RAM** | ✅ Sí (Resize) | ✅ Puede ser necesario |
| Añadir un Data Disk | ❌ No* | ❌ No |
| Quitar un Data Disk | ❌ No* | ❌ No |
| Cambiar el tamaño del OS Disk | ✅ Sí | ✅ Sí |
| Cambiar el tamaño de un Data Disk | ❌ No** | ❌ No |
| Cambiar la VNet | ❌ No posible | Debe recrearse la VM |
| Cambiar la Subnet | ❌ No | ✅ Sí |
| Cambiar la NIC principal | ❌ No posible | Debe recrearse la VM |
| Añadir una NIC secundaria | ❌ No*** | ❌ No |
| Eliminar una NIC secundaria | ❌ No | ✅ Sí |
| Cambiar el NSG | ❌ No | ❌ No |
| Cambiar las Tags | ❌ No | ❌ No |
| Habilitar Managed Identity | ❌ No | ❌ No |
| Habilitar Boot Diagnostics | ❌ No | ❌ No |


\* Si la VM y el sistema operativo soportan **Hot Add/Remove**.

\** Online Resize soportado para la mayoría de discos administrados.

\*** Siempre que el tamaño de la VM soporte varias NICs.

---

# Stop vs Shutdown

## Stop (Deallocate) vs Shutdown

No es lo mismo apagar una VM desde el sistema operativo que detenerla desde Azure.

| Acción | ¿Qué ocurre con el hardware? | ¿Se sigue pagando el cómputo? |
|--------|-------------------------------|-------------------------------|
| **Shutdown desde Windows/Linux** | La VM se apaga, pero **Azure mantiene reservado el servidor físico (host)** donde se ejecutaba. | **Sí.** Se sigue facturando el cómputo porque el hardware continúa reservado. |
| **Stop (Deallocate) desde Azure** | Azure **libera el servidor físico (host)** y la VM deja de tener recursos de cómputo asignados. | **No.** Se deja de facturar el cómputo. Solo se sigue pagando el almacenamiento (discos, snapshots, etc.). |

### Regla para el AZ-104

- **Shutdown** → La VM está apagada, **pero el hardware sigue reservado** y **se sigue pagando el cómputo**.
- **Stop (Deallocate)** → Azure **libera el hardware**, **no se paga el cómputo** y la VM podrá iniciarse posteriormente en el mismo o en otro host físico.

---

# Preguntas típicas del AZ-104

✅ Una VM puede cambiar de tamaño después de su creación.

✅ Si el nuevo tamaño no está disponible en el clúster actual, será necesario realizar un **Stop (Deallocate)**.

✅ Cambiar el **NSG**, las **Tags** o habilitar una **Managed Identity** no requiere reiniciar la VM.

✅ No es posible cambiar la **VNet** ni la **NIC principal** de una VM existente; hay que recrearla.

---

# Regla para el examen

- **Resize** → Puede requerir **Stop (Deallocate)**.
- **Cambios de configuración (NSG, Tags, Managed Identity...)** → No requieren reinicio.
- **Cambiar VNet o NIC principal** → No es posible; hay que recrear la VM.
---

# 7. Redeploy

La opción **Redeploy** mueve la máquina virtual a un nuevo host físico.

Se utiliza para solucionar problemas relacionados con el host de Azure.

No modifica:

- La IP privada estática.
- Los discos.
- La configuración de la VM.

---

# 8. Reapply

La opción **Reapply** vuelve a aplicar la configuración de Azure sobre la máquina virtual.

Se utiliza cuando existen problemas de sincronización entre Azure y el host.

No mueve la VM a otro servidor físico.

---

# 9. VM Extensions

Las **VM Extensions** permiten instalar y configurar software automáticamente después del despliegue.

Ejemplos:

- Azure Monitor Agent.
- Custom Script Extension.
- Desired State Configuration (DSC).
- Dependency Agent.

---

# 10. Azure VM Backup

Las máquinas virtuales se protegen mediante:

**Recovery Services Vault**

Azure Backup permite:

- Restaurar la VM completa.
- Restaurar únicamente los discos.
- Recuperar archivos individuales (**File Recovery**).

---

# 11. VM Scale Sets (VMSS)

Los **Virtual Machine Scale Sets** permiten crear un conjunto de máquinas virtuales idénticas.

Características:

- Escalado automático.
- Alta disponibilidad.
- Balanceo mediante Azure Load Balancer.

Muy utilizados para aplicaciones web.
Los **Virtual Machine Scale Sets (VMSS)** permiten crear y administrar un conjunto de **máquinas virtuales idénticas**.

Todas las instancias comparten la misma configuración (imagen, tamaño, discos, extensiones, etc.) y Azure puede aumentar o reducir automáticamente el número de VMs según la demanda.

---

## Características

- Escalado automático (Autoscale).
- Alta disponibilidad.
- Balanceo mediante **Azure Load Balancer**.
- Administración centralizada.
- Muy utilizados para aplicaciones web y APIs.

---

## Funcionamiento

```text
Internet
      │
      ▼
Azure Load Balancer
      │
 ┌────┼────┐
 │    │    │
VM1  VM2  VM3
 (VM Scale Set)
```

El **Load Balancer** distribuye las conexiones entre las instancias del Scale Set.

---

## Escalado automático "Autoscale"

Azure Monitor puede aumentar o disminuir el número de instancias según reglas configuradas.

Ejemplos:

- CPU > 75% durante 10 minutos → **+1 VM**
- CPU < 25% durante 10 minutos → **-1 VM**

También es posible escalar según:

- Memoria (mediante Azure Monitor Agent)
- Número de solicitudes HTTP
- Cola de Azure Storage
- Horario (Schedule)

---

## Modos de orquestación

### Uniform (más utilizado)

- Todas las VMs son idénticas.
- Azure administra automáticamente las instancias.
- Recomendado para aplicaciones escalables.

### Flexible

- Permite VMs con configuraciones diferentes.
- Mayor flexibilidad para distintos tipos de cargas.

---

## Alta disponibilidad

Las instancias pueden distribuirse entre:

- Fault Domains
- Update Domains
- Availability Zones (según la configuración)

Esto reduce el impacto de fallos de hardware o mantenimiento.

---

## Preguntas típicas del AZ-104

✅ VMSS permite **escalar horizontalmente** (añadir o eliminar VMs).

❌ No aumenta la CPU o la memoria de una VM existente (eso es **Resize**, es decir, escalado vertical).

✅ Normalmente se utiliza junto con un **Azure Load Balancer**.

✅ Azure Monitor puede desencadenar el escalado automático mediante reglas.

---

## Escalado horizontal vs vertical

| Tipo | Acción |
|-------|--------|
| **Horizontal (Scale Out / In)** | Añadir o eliminar máquinas virtuales |
| **Vertical (Scale Up / Down)** | Cambiar el tamaño (SKU) de una VM existente |

---

## Regla para el AZ-104

- **VM Scale Sets = Escalado horizontal.**
- **Resize de una VM = Escalado vertical.**
- Los **VMSS** suelen utilizarse junto con un **Azure Load Balancer** y reglas de **Autoscale** para soportar aplicaciones con carga variable.
  
---

# 12. Autoscale

VM Scale Sets pueden escalar según:

- CPU.
- Memoria (Azure Monitor).
- Número de instancias.
- Horarios programados.

El escalado puede ser:

- Horizontal (más VMs).
- Automático.

Los **VM Scale Sets** pueden escalar automáticamente mediante **Azure Monitor Autoscale**.

El escalado se basa en reglas configurables, por ejemplo:

- Uso de CPU.
- Uso de memoria (requiere Azure Monitor Agent).
- Número de solicitudes.
- Longitud de una cola de Azure Storage.
- Horarios programados (Schedule).

Ejemplos:

- CPU > 75% durante 10 minutos → **Añadir 1 VM (Scale Out)**.
- CPU < 25% durante 10 minutos → **Eliminar 1 VM (Scale In)**.

---

## Tipos de escalado

### Escalado horizontal (Scale Out / Scale In)

Consiste en **añadir o eliminar máquinas virtuales**.

Es el tipo de escalado utilizado por los **VM Scale Sets**.

Ejemplo:

```text
3 VMs

↓

5 VMs
```

---

### Escalado vertical (Scale Up / Scale Down)

Consiste en **cambiar el tamaño (SKU) de una VM** para aumentar o disminuir:

- vCPU
- Memoria RAM

No lo realiza un VM Scale Set; se hace mediante un **Resize** de la VM.

---

## Regla para el AZ-104

- **VM Scale Sets** → Escalado **horizontal**.
- **Autoscale** → Automatiza el escalado horizontal según métricas o horarios.
- **Resize** → Escalado **vertical** (cambio de tamaño de una VM).
---

# 13. Cuotas (Quotas)

Azure limita el número de **vCPUs** por región y suscripción.

Si no existen suficientes vCPUs disponibles:

↓

No será posible desplegar nuevas máquinas virtuales.

La cuota puede solicitarse a Microsoft.

---

# 14. Actualizaciones

Azure puede reiniciar una VM durante operaciones de mantenimiento planificado.

Para minimizar el impacto se recomienda utilizar:

- Availability Sets.
- Availability Zones.
- VM Scale Sets.

---

# 15. Buenas prácticas

Microsoft recomienda:

- Utilizar **Managed Disks**.
- Proteger las VMs mediante **Azure Backup**.
- No asignar **Public IP** cuando se utilice **Azure Bastion**.
- Utilizar **Availability Zones** cuando estén disponibles.
- Utilizar **VM Scale Sets** para aplicaciones escalables.

---

# Preguntas trampa del AZ-104

✅ **Availability Sets** protegen frente a fallos de hardware y mantenimiento.

✅ **Availability Zones** protegen frente a la pérdida de un datacenter.

✅ **Redeploy** mueve la VM a otro host físico.

✅ **Reapply** no mueve la VM; solo reaplica la configuración.

✅ Las **VM Extensions** permiten instalar software automáticamente.

✅ Los backups de VMs utilizan un **Recovery Services Vault**.

✅ **VM Scale Sets** permiten el escalado automático.

✅ Las cuotas de Azure limitan el número de **vCPUs** disponibles por región.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Virtual Machine | IaaS |
| Availability Set | Fault Domains + Update Domains |
| Availability Zone | Protege un datacenter |
| Managed Disk | Recomendado |
| Premium SSD | Alto rendimiento |
| Redeploy | Nuevo host físico |
| Reapply | Reaplica configuración |
| VM Extension | Instala software |
| Azure Backup | Recovery Services Vault |
| VM Scale Set | Escalado automático |
| Autoscale | CPU / Métricas / Horarios |
| Quotas | Límite de vCPUs |
