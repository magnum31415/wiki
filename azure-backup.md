[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 📑 Índice

- [🔄 Opciones de Azure Backup](#-opciones-de-azure-backup)
- [📊 Comparativa rápida](#-comparativa-rápida)
- [🧠 Regla mental AZ-305](#-regla-mental-az-305)
- [🏁 En una frase](#-en-una-frase)

- [🧩 1️⃣ Azure VM Backup](#-1️⃣-azure-vm-backup)
- [🧩 2️⃣ Azure Backup Agent (MARS)](#-2️⃣-azure-backup-agent-mars-microsoft-azure-recovery-services)
- [🧩 3️⃣ Azure Backup Server (MABS)](#-3️⃣-azure-backup-server-mabs)
- [🧩 4️⃣ System Center Data Protection Manager (DPM)](#-4️⃣-system-center-data-protection-manager-dpm)
- [🧩 5️⃣ Azure SQL Backup (PaaS)](#-5️⃣-azure-sql-backup-paas)
- [🧩 6️⃣ SAP HANA Backup en Azure](#-6️⃣-sap-hana-backup-en-azure)
- [🧩 7️⃣ Azure Files Backup](#-7️⃣-azure-files-backup)

- [🔄 MARS vs MABS (Azure Backup)](#-mars-vs-mabs-azure-backup)
- [🧩 1️⃣ MARS (Microsoft Azure Recovery Services Agent)](#-1️⃣-mars-microsoft-azure-recovery-services-agent)
- [🧩 2️⃣ MABS (Microsoft Azure Backup Server)](#-2️⃣-mabs-microsoft-azure-backup-server)
- [📊 Comparativa clara](#-comparativa-clara)
- [🧠 Regla mental AZ-305](#-regla-mental-az-305)
- [🏁 En una frase](#-en-una-frase)


# 🔄 Opciones de Azure Backup

Azure Backup es el servicio gestionado de copias de seguridad en Azure.  
Dependiendo del tipo de recurso, existen distintas **opciones / métodos de protección**.

---

# 🧩 1️⃣ Azure VM Backup

# 📊 Comparativa rápida

| Opción | Protege | On-Prem | Azure | VM completa | Nivel |
|--------|----------|----------|--------|-------------|-------|
| Azure VM Backup | Azure VMs | ❌ | ✅ | ✅ | PaaS |
| MARS Agent | Archivos Windows | ✅ | ✅ | ❌ | Básico |
| MABS | VMs + Apps | ✅ | ✅ | ✅ | Intermedio |
| DPM | Workloads enterprise | ✅ | Opcional | ✅ | Enterprise |
| SQL PaaS Backup | Azure SQL | ❌ | ✅ | N/A | Integrado |
| Azure Files Backup | File Shares | ❌ | ✅ | N/A | Integrado |

---

# 🧠 Regla mental AZ-305

- VM en Azure → Azure VM Backup
- Servidor Windows simple on-prem → MARS
- Entorno híbrido complejo → MABS
- SQL PaaS → Backup integrado
- DR con failover → Azure Site Recovery (no Backup)

---

# 🏁 En una frase

Azure Backup tiene distintas implementaciones según el tipo de recurso: VM nativa, agente MARS, servidor MABS o backups integrados en servicios PaaS.

---
## 🔎 Qué es
Backup nativo de **máquinas virtuales en Azure**.

## 📦 Qué protege
- VM completa
- Discos (OS + Data)
- Configuración

## 🎯 Características
- Snapshot + backup incremental
- Application-consistent (VSS en Windows)
- Restauración de:
  - VM completa
  - Discos
  - Archivos individuales

👉 Es la opción estándar para VMs en Azure.

---

# 🧩 2️⃣ Azure Backup Agent (MARS) "Microsoft Azure Recovery Services"

## 🔎 Qué es
Agente instalado en **Windows Server / Windows client**.

## 📦 Qué protege
- Archivos y carpetas
- System State

## ⚠ Limitaciones
- Solo Windows
- No protege VM completa
- No hace DR

👉 Ideal para servidores on-prem simples sin infraestructura adicional.

---

# 🧩 3️⃣ Azure Backup Server (MABS)

## 🔎 Qué es
Versión ligera de **System Center DPM** incluida sin coste adicional con Azure Backup.

## 📦 Qué protege
- VMs Hyper-V
- VMware
- SQL Server
- Exchange
- SharePoint
- Archivos

## 🎯 Características
- Backup local + envío a Azure
- Protección más avanzada que MARS
- Soporta entornos enterprise

👉 Ideal para entornos híbridos complejos.

---

# 🧩 4️⃣ System Center Data Protection Manager (DPM)

## 🔎 Qué es
Solución enterprise de Microsoft para backup on-prem.

## 📦 Qué protege
- Workloads Microsoft completos
- VMs
- Aplicaciones empresariales

👉 Puede integrarse con Azure Backup como destino externo.

---

# 🧩 5️⃣ Azure SQL Backup (PaaS)

## 🔎 Qué es
Backup automático integrado en:

- Azure SQL Database
- SQL Managed Instance

## 🎯 Características
- Backups automáticos
- Retención configurable
- Point-in-time restore
- LTR (Long Term Retention)

👉 No requiere MARS ni agente.

---

# 🧩 6️⃣ SAP HANA Backup en Azure

Protección específica para:
- SAP HANA en Azure VMs

Integrado con Azure Backup.

---

# 🧩 7️⃣ Azure Files Backup

Backup nativo de:
- Azure File Shares

Sin necesidad de agente.

---
# 📋 1️⃣ Backup Policy

Una **Backup Policy** define:

> Cómo y cuándo se hacen los backups  
> Cuánto tiempo se conservan

Es un objeto reutilizable que se asocia a recursos.

## Incluye:

- Backup Schedule (frecuencia)
- Retention Policy (retención)

## Ejemplo:

- Backup diario a las 22:00
- Retención:
  - 30 días
  - 12 meses
  - 5 años

## Punto importante examen

- Una policy puede aplicarse a múltiples recursos.
- Cambiar la policy afecta a todos los recursos asociados.
- No almacena datos, solo define configuración.

---

# ⏰ 2️⃣ Backup Schedule

El **Backup Schedule** forma parte de la Backup Policy.

Define:

> Cuándo se ejecuta el backup

Puede ser:

- Diario
- Semanal
- (En algunos workloads) Horario

Ejemplo:

- Todos los días a las 23:00
- Cada domingo a las 02:00

⚠️ Importante:
Schedule ≠ Retention  
Uno define cuándo se ejecuta  
El otro cuánto tiempo se guarda

---

# 🗂 3️⃣ Backup Logs

Los **Backup Logs** permiten:

- Ver si el backup fue exitoso
- Detectar fallos
- Auditar ejecuciones
- Diagnosticar problemas

Se pueden consultar en:

- Recovery Services Vault
- Azure Monitor
- Log Analytics (si está habilitado)

Información típica:

- Inicio/fin del job
- Estado (Completed / Failed)
- Tamaño del backup
- Duración

---

# 🏗 4️⃣ Backup Infrastructure

La **Backup Infrastructure** es la arquitectura que hace posible el backup.

Incluye:

## 🔹 Recovery Services Vault
Contenedor lógico donde se almacenan:

- Backup policies
- Backup jobs
- Restore points
- Configuración

## 🔹 Backup Agents / Extensions

Dependiendo del workload:

- VM Extension (para Azure VMs)
- MARS Agent (on-premises)
- Azure Backup Server
- SQL Backup Extension

## 🔹 Storage gestionado por Azure

- Azure gestiona almacenamiento
- Replicación configurable:
  - LRS
  - GRS
  - ZRS

---

# 🧠 Relación entre los conceptos

Recurso (VM / SQL / Files)
        ↓
Backup Policy
        ↓
Backup Schedule
        ↓
Backup Job
        ↓
Recovery Services Vault
        ↓
Backup Logs

---

# 🎯 Puntos típicos de examen (AZ-104 / AZ-305)

✔ Backup Policy define frecuencia + retención  
✔ Schedule define cuándo  
✔ Vault es obligatorio  
✔ Backup no usa la subnet GatewaySubnet  
✔ GRS permite restauración regional  

---

# 📌 Resumen rápido

| Concepto | Qué es | Qué controla |
|----------|--------|--------------|
| Backup Policy | Configuración lógica | Frecuencia + Retención |
| Backup Schedule | Parte de la policy | Momento de ejecución |
| Backup Logs | Registro de ejecuciones | Estado y auditoría |
| Backup Infrastructure | Arquitectura subyacente | Dónde y cómo se almacena |


---



---

