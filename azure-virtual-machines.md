[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Virtual Machines

Azure Virtual Machines are cloud-based computing resources that can be used to run a wide variety of applications.
They are scalable, reliable, secure, and can be provisioned quickly and easily. 
It is also available in a variety of sizes and configurations, so you can choose the right VM for your needs.

## Disk cache

These are the recommended disk cache settings for data disks:
- **None** – configure host-cache as None for write-only and write-heavy disks.
- **ReadOnly** – configure host-cache as ReadOnly for read-only and read-write disks.
- **ReadWrite** – configure host-cache as ReadWrite only if your application properly handles writing cached data to persistent disks when needed.



## Availability Set

| Concepto          | Protege contra                | Nivel                   |
| ----------------- | ----------------------------- | ----------------------- |
| Fault Domain      | Fallo físico de rack/hardware | Dentro de un datacenter |
| Update Domain     | Reinicios planificados        | Dentro de un datacenter |
| Availability Zone | Fallo de datacenter completo  | Dentro de región        |
| Región secundaria | Fallo regional                | Entre regiones          |


- **Fault Domains (FD)** = protegen ante fallos físicos (rack / alimentación / red).
- **Update Domains (UD)** = “lotes” lógicos para mantenimiento planificado (Azure reinicia un UD a la vez).

### 🔎 Fault Domain (FD)

**Fault Domains son un concepto de Availability Set para distribuir VMs en distintos racks físicos y evitar un único punto de fallo.**

Un Fault Domain (FD) es un grupo de recursos que comparten:

- La misma alimentación eléctrica
- El mismo rack físico
- El mismo switch de red

👉 Si falla el rack o el hardware físico, solo afecta a ese Fault Domain.
###  🔄 Update Domain (UD) 


Un **Update Domain (UD)** es un grupo lógico de recursos (normalmente VMs dentro de un Availability Set) que Azure reinicia **al mismo tiempo durante tareas de mantenimiento planificado**.

👉 Se usa para evitar que todas las máquinas se reinicien a la vez.

** 🏗 ¿Dónde se utiliza?**

Principalmente en:

- **Availability Sets**
- Máquinas virtuales (VMs)

Cuando creas un Availability Set, Azure distribuye las VMs entre:

- Fault Domains (fallos físicos)
- Update Domains (mantenimiento planificado)

**⚙️ ¿Qué protege?**

Protege contra:

- Reinicios por actualizaciones del host
- Mantenimiento del hypervisor
- Parches de infraestructura Azure

No protege contra:

- Fallos físicos (eso es Fault Domain)
- Fallos de datacenter (eso es Availability Zone)



### Ejemplo

Tenemos:
- 10 VMs
- 2 FD
- 3 UD

Eso forma una “rejilla” de 2 × 3 = 6 celdas (cada celda es un par FD/UD).


| UD \ FD          |   FD0 |   FD1 | Total por UD |
| ---------------- | ----: | ----: | -----------: |
| UD0              |     2 |     2 |            4 |
| UD1              |     2 |     1 |            3 |
| UD2              |     1 |     2 |            3 |
| **Total por FD** | **5** | **5** |       **10** |


#### 1) ¿Cómo se reparten por Update Domains (UD)?**

Azure intenta repartir lo más equilibrado posible.
- Con 10 VMs y 3 UDs:
  - 10 / 3 = 3 y sobran 1
  - → distribución típica: 4, 3, 3 (un UD tendrá 4 VMs)

Ejemplo:
````
UD0: 4 VMs
UD1: 3 VMs
UD2: 3 VMs
````

**✅ Mantenimiento planificado (planned maintenance)**
- Azure “toca” (reinicia/mueve) un UD entero.
  - El peor caso es que le toque el UD que tiene más VMs (4).
  - ➡️ VMs disponibles mínimo = 10 − 4 = 6
 
 #### 2) ¿Y cómo se reparten entre Fault Domains (FD)?
Dentro de cada UD, Azure también intenta balancear entre los 2 FDs.

Una forma equilibrada (ejemplo válido) sería:

| UD \ FD          |   FD0 |   FD1 | Total por UD |
| ---------------- | ----: | ----: | -----------: |
| UD0              |     2 |     2 |            4 |
| UD1              |     2 |     1 |            3 |
| UD2              |     1 |     2 |            3 |
| **Total por FD** | **5** | **5** |       **10** |

Esto cumple:

- Balance global por FD: 5 y 5
- Balance por UD: 4 / 3 / 3

**Nota: Azure no te garantiza exactamente “esta” tabla, pero sí el principio de “lo más equilibrado posible”. 
Para el cálculo de planned maintenance, lo que manda es el UD con más VMs.**
