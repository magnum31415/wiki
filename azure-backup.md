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


---



---

