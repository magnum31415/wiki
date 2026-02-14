[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# Storage

Hay tres conceptos distintos que se cruzan:

-🔹 Storage Account
-🔹 Access Tier (Hot/Cool/Cold/Archive)
-🔹 Redundancia (LRS, ZRS, GRS, etc.)

---
## Tipo de Storage Account (SKU funcional)

Esto define qué tipo de almacenamiento puedes usar y con qué rendimiento.


| Tipo de cuenta                           | Para qué sirve               | Rendimiento | Soporta tiers Hot/Cool/Cold/Archive | Redundancia soportada |
|------------------------------------------|------------------------------|------------|-------------------------------------|-----------------------|
| **Standard – General Purpose v2 (GPv2)** | Blob, File, Queue, Table     | HDD        | ✅ Sí                                | LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS |
| **Premium – Block Blobs**                | Blob de alto rendimiento     | SSD        | ❌ No (solo Hot implícito)           | LRS, ZRS |
| **Premium – File Shares**                | Azure Files alto rendimiento | SSD        | ❌ No                                | LRS, ZRS |
| **Premium – Page Blobs**                 | Discos de VM (VHD)           | SSD        | ❌ No                                | LRS |
| *(Legacy)* GPv1                          | Antiguo                      | HDD        | Limitado                             | LRS, GRS, RA-GRS |


Standard (GPv2) → hasta 5 PiB (Pebibytes) por Storage Account


| Tipo de cuenta | Tipo de datos que soporta | Blob Versioning | Soft Delete | Snapshots | Lifecycle Management | Casos de uso típicos |
|---------------|----------------------------|-----------------|------------|-----------|---------------------|----------------------|
| **Standard – General Purpose v2 (GPv2)** | Blob, File, Queue, Table | ✅ Sí (Blob) | ✅ Sí (Blob & File) | ✅ Sí (Blob) | ✅ Sí (Hot/Cool/Cold/Archive + Delete) | Workloads generales, backup, data lake, archivos híbridos, optimización de costes |
| **Premium – Block Blobs** | Blob (alto rendimiento) | ✅ Sí | ✅ Sí | ✅ Sí | ❌ No (sin Archive/Cold) | Streaming, media, ingestión masiva, alto throughput |
| **Premium – File Shares** | Azure Files (SMB/NFS) | ❌ No | ✅ Sí | ❌ No (usa backup nativo) | ❌ No | File servers empresariales, lift-and-shift de shares |
| **Premium – Page Blobs** | Page Blobs (VHD discos VM) | ❌ No | ❌ No | ❌ No | ❌ No | Discos de máquinas virtuales (IaaS) |
| **(Legacy) GPv1** | Blob, File, Queue, Table | ❌ Limitado | ❌ Limitado | ✅ Básico | ❌ No | Entornos antiguos (no recomendado para nuevos despliegues) |



| Necesidad                                   | Elección típica                 |
| ------------------------------------------- | ------------------------------- |
| Coste optimizado por temperatura            | GPv2                            |
| Rendimiento SSD extremo                     | Premium                         |
| Retención legal / protección contra borrado | Versioning + Soft Delete (GPv2) |
| Automatización de transición de datos       | Lifecycle (solo GPv2)           |

📌 Importante:

- Solo Standard GPv2 soporta Hot / Cool / Cold / Archive
- Las Premium son para rendimiento, no para optimización por acceso

## Redundancia en Azure Storage

| Redundancia                           | Nº réplicas              | ¿Dónde se replica?                                       | Protege contra                  | Acceso a región secundaria       | Caso típico                          |
| ------------------------------------- | ------------------------ | -------------------------------------------------------- | ------------------------------- | -------------------------------- | ------------------------------------ |
| **LRS** (Locally Redundant Storage)   | 3                        | Mismo datacenter, misma región                           | Fallo de disco / rack local     | ❌ No                             | Datos no críticos, entorno DEV       |
| **ZRS** (Zone Redundant Storage)      | 3                        | 3 Availability Zones dentro de la misma región           | Caída de zona completa          | ❌ No                             | Alta disponibilidad regional         |
| **GRS** (Geo-Redundant Storage)       | 6 (3 + 3)                | 3 en región primaria + 3 en región secundaria emparejada | Caída total de región           | ❌ No (solo tras failover manual) | DR entre regiones                    |
| **RA-GRS** (Read-Access GRS)          | 6 (3 + 3)                | Igual que GRS                                            | Caída de región                 | ✅ Sí (read-only)                 | Apps que leen desde secundaria       |
| **GZRS** (Geo-Zone Redundant Storage) | 6 (3 ZRS + 3 secundaria) | 3 zonas en primaria + 3 en región secundaria             | Caída de zona + caída de región | ❌ No (solo tras failover)        | Workloads críticos empresariales     |
| **RA-GZRS**                           | 6                        | Igual que GZRS                                           | Caída zona + región             | ✅ Sí (read-only)                 | Alta disponibilidad + lectura global |


![ZRS](./img/azure/azure-zone-redundant-storage.png)


## Access tiers

| Tier    | Frecuencia   | Coste storage | Coste acceso | Latencia   | Retención mínima |
|---------|-------------|--------------|-------------|------------|------------------|
| Hot     | Alta        | Alto         | Bajo        | Inmediata  | 0 días           |
| Cool    | Media-baja  | Medio        | Medio       | Inmediata  | 30 días          |
| Cold    | Baja        | Bajo         | Alto        | Inmediata  | 90 días          |
| Archive | Muy baja    | Muy bajo     | Muy alto    | Horas      | 180 días         |

Cambios entre tiers (muy preguntado)

 ````
 ✔ Hot ↔ Cool ↔ Cold → inmediato
 ✔ Archive → Hot/Cool → rehydration (horas)
 ````

**política automática de ciclo de vida (Lifecycle Management) aplicada sobre blobs.**

````
Día 30   → Mover a Cool
Día 90   → Mover a Cold
Día 180  → Mover a Archive
Día 3650 → Delete
````
