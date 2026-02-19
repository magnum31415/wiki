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



## AZURE Availability Set

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


# ⚖ Availability Set vs Virtual Machine Scale Sets VMSS

---

# 🧠 Concepto básico

Availability Set
→ Alta disponibilidad básica para pocas VMs

VM Scale Set
→ Alta disponibilidad + escalado automático

---

# 📦 1️⃣ Availability Set

📍 Qué es:
Agrupa VMs para distribuirlas en distintos Fault Domains y Update Domains.

📍 Qué hace:
- Evita que todas las VMs caigan por mantenimiento o fallo físico.
- No escala automáticamente.
- No gestiona VMs como grupo dinámico.

````mermaid

flowchart TB

    subgraph AVSET["Availability Set"]

        subgraph FD1["Fault Domain 1"]
            VM1[VM 1]
            VM2[VM 2]
        end

        subgraph FD2["Fault Domain 2"]
            VM3[VM 3]
            VM4[VM 4]
        end

    end

    FD1 --- FD2

````

📍 Pensado para:
- 2–3 VMs manuales
- Arquitectura clásica
- Workloads estables

📍 No permite:
- Autoscaling
- Gestión masiva
- Escalado dinámico

---

# 📦 2️⃣ Virtual Machine Scale Sets VMSS

📍 Qué es:
Conjunto de VMs idénticas que se gestionan como una unidad.

📍 Qué hace:
- Escalado automático (horizontal)
- Integración con Load Balancer
- Distribución en zonas
- Gestión centralizada

Cuando creas un VM Scale Set puedes elegir:

1️⃣ Regional (una sola zona implícita)  
2️⃣ Zonal (una zona específica)  
3️⃣ Multi-zone (distribuido entre varias Availability Zones)

Depende de cómo lo configures.

----

````mermaid

flowchart LR

    %% REGIONAL VMSS
    subgraph REGIONAL_VMSS["VM Scale Set - Regional"]
        direction TB
        R1[Instancia VM 1]
        R2[Instancia VM 2]
        R3[Instancia VM 3]

        R1 --- R2
        R2 --- R3

        RNote[Distribuidas en Fault Domains<br>Misma región<br>Misma zona implícita]
    end

    %% MULTI ZONE VMSS
    subgraph MULTI_ZONE_VMSS["VM Scale Set - Multi Zone"]
        direction TB
        
        subgraph Z1["Availability Zone 1"]
            Z1A[VM]
        end

        subgraph Z2["Availability Zone 2"]
            Z2A[VM]
        end

        subgraph Z3["Availability Zone 3"]
            Z3A[VM]
        end

        Z1A --- Z2A
        Z2A --- Z3A
    end

    %% Forzar layout horizontal
    REGIONAL_VMSS --- MULTI_ZONE_VMSS
````


---

## 📊 Modos posibles

| Tipo de despliegue | Alta disponibilidad | Multi-AZ |
|--------------------|--------------------|----------|
| Regional | Fault Domains internos | ❌ |
| Zonal | En una sola AZ | ❌ |
| Multi-zone | Distribuido en varias AZ | ✅ |


##📍 Pensado para:
- Aplicaciones con picos de carga
- Web frontends
- Microservicios
- Workloads cloud-native

📍 Permite:
- Autoscale con Azure Monitor
- Escalar manual o automático
- Rolling upgrades
- Integración con AKS

---

# 📊 Comparativa clara

| Característica | Availability Set | VM Scale Set |
|---------------|-----------------|--------------|
| Alta disponibilidad | ✅ | ✅ |
| Fault Domains | ✅ | ✅ |
| Update Domains | ✅ | ✅ |
| Escalado automático | ❌ | ✅ |
| Gestión como grupo | ❌ | ✅ |
| Integración Load Balancer | Manual | Integrado |
| Ideal para producción dinámica | ❌ | ✅ |
| Soporta miles de VMs | ❌ | ✅ |

---

# 🎯 Preguntas típicas de examen

Si el requisito dice:

"Evitar downtime por mantenimiento"
→ Availability Set

"Escalar automáticamente según CPU"
→ VM Scale Set

"Aplicación web con picos de tráfico"
→ VM Scale Set

"Solo necesito 2 VMs redundantes"
→ Availability Set

---

# 🧠 Diferencia arquitectónica clave

Availability Set:
Distribuye VMs que tú creas manualmente.

Scale Set:
Crea y gestiona automáticamente múltiples instancias idénticas.

---

# 🔄 Relación con AKS

AKS usa internamente:

→ VM Scale Sets para sus nodos.

---

# 📌 Regla mental examen

Availability Set = Alta disponibilidad básica  
Scale Set = Alta disponibilidad + Escalado automático  

---

# 🔥 Resumen ultra corto

Availability Set:
Evita caída simultánea.

VM Scale Set:
Evita caída + escala automáticamente.

---

# Tipos de VM más importantes en Azure


| Serie        | Características principales             | CPU / RAM perfil                        | Casos ideales                                                    | No ideal para           |
| ------------ | --------------------------------------- | --------------------------------------- | ---------------------------------------------------------------- | ----------------------- |
| **B-series** | Burstable (modelo de créditos CPU)      | CPU baja base + picos temporales        | Dev/Test, aplicaciones con uso intermitente, bajo coste          | Producción crítica 24/7 |
| **D-series** | Propósito general, buen balance CPU/RAM | Balanceado                              | Aplicaciones empresariales, APIs, servidores web, bases pequeñas | HPC, ML pesado          |
| **N-series** | GPUs (NVIDIA)                           | CPU + GPU                               | Machine Learning, IA, renderizado, inferencia, simulación        | Workloads sin GPU       |
| **H-series** | High Performance Computing              | CPU muy alta, baja latencia, InfiniBand | Simulación científica, modelado financiero, cargas HPC           | Aplicaciones estándar   |


| Si la pregunta dice…       | Piensa en… |
| -------------------------- | ---------- |
| Low cost / intermittent    | B-series   |
| Balanced workload          | D-series   |
| AI / GPU / rendering       | N-series   |
| Scientific computing / HPC | H-series   |
