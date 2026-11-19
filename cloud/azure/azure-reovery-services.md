[Azure](azure.md)

# 📑 Índice

- [🔄 Azure Recovery Services vs Azure Site Recovery](#-azure-recovery-services-vs-azure-site-recovery)
- [🧩 1️⃣ Recovery Services Vault](#-1️⃣-recovery-services-vault)
- [🧩 2️⃣ Azure Site Recovery (ASR)](#-2️⃣-azure-site-recovery-asr)
- [📊 Comparativa rápida](#-comparativa-rápida)
- [🧠 Diferencia conceptual clave](#-diferencia-conceptual-clave)
- [🎯 Regla mental AZ-305](#-regla-mental-az-305)
- [🏁 En una frase](#-en-una-frase)
- [🔄 Azure Backup vs Recovery Services Vault](#-azure-backup-vs-recovery-services-vault)
- [🧩 Azure Backup](#-azure-backup)
- [🧩 Recovery Services Vault](#-recovery-services-vault)
- [📊 Comparativa clara](#-comparativa-clara)
- [🧠 Cómo funciona en la práctica](#-cómo-funciona-en-la-práctica)
- [🎯 Regla mental AZ-305 (Backup)](#-regla-mental-az-305-1)
- [🏁 En una frase (Backup)](#-en-una-frase-1)


# 🔄 Azure Recovery Services vs Azure Site Recovery

Muchos los confunden, pero **no son lo mismo**.

---

# 🧩 1️⃣ Recovery Services Vault

## 🔎 ¿Qué es?

Es el **contenedor** donde se almacenan:

- Backups
- Configuraciones de recuperación
- Metadatos de protección

👉 Es el “vault” (baúl) donde viven los backups.

---

## 🎯 Para qué se usa

- Azure Backup (VMs, SQL, Files, etc.)
- Azure Site Recovery (replicación y DR)

⚠ No hace backup ni replicación por sí solo.  
Es solo el contenedor lógico.

---

# 🧩 2️⃣ Azure Site Recovery (ASR)

## 🔎 ¿Qué es?

Servicio de **Disaster Recovery (DR)**.

Replica máquinas:

- On-prem → Azure
- Azure → Azure (entre regiones)

---

## 🎯 Qué hace

- Replica discos
- Permite failover
- Permite failback
- Orquesta recuperación

👉 Es para continuidad de negocio (BCDR).

---

# 📊 Comparativa rápida

| Característica | Recovery Services Vault | Azure Site Recovery |
|---------------|--------------------------|---------------------|
| ¿Es un servicio? | ❌ No (es contenedor) | ✅ Sí |
| Hace backup | ❌ No directamente | ❌ No |
| Replica VMs | ❌ No | ✅ Sí |
| Permite failover | ❌ No | ✅ Sí |
| Se usa para Azure Backup | ✅ Sí | ❌ No |
| Se usa para DR | Soporta metadatos | ✅ Sí |

---

# 🧠 Diferencia conceptual clave

- **Recovery Services Vault** → Dónde se guardan backups y configuraciones
- **Azure Site Recovery** → Servicio que replica y permite failover

---

# 🎯 Regla mental AZ-305

Si lees:

- “Backup”
- “Retención 30 días”
- “Restore a point in time”

👉 Azure Backup + Recovery Services Vault

Si lees:

- “Disaster Recovery”
- “Failover”
- “Replication between regions”
- “Business continuity”

👉 Azure Site Recovery

---

# 🏁 En una frase

Recovery Services Vault es el contenedor.  
Azure Site Recovery es el motor de Disaster Recovery.


# 🔄 Azure Backup vs Recovery Services Vault

Muchos los confunden, pero no son lo mismo.

---

# 🧩 Azure Backup

## 🔎 ¿Qué es?

Servicio de **backup gestionado** en Azure.

Permite proteger:

- Azure VMs
- Azure SQL
- SAP HANA
- Azure Files
- On-premises (vía agente o MARS)
- Windows Server / System Center

👉 Es el servicio que realiza las copias de seguridad.

---

## 🎯 Qué hace

- Crea backups automáticos
- Gestiona retención (días, meses, años)
- Permite restore (PITR)
- Cifra los datos
- Gestiona políticas de backup

---

# 🧩 Recovery Services Vault

## 🔎 ¿Qué es?

Es el **contenedor lógico** donde se almacenan:

- Backups
- Políticas de retención
- Metadatos de protección
- Configuración de Azure Site Recovery

👉 Es el “baúl” donde vive Azure Backup.

---

# 📊 Comparativa clara

| Característica | Azure Backup | Recovery Services Vault |
|---------------|--------------|--------------------------|
| ¿Es un servicio? | ✅ Sí | ❌ No (es contenedor) |
| Realiza copias de seguridad | ✅ Sí | ❌ No |
| Almacena backups | ❌ No directamente | ✅ Sí |
| Gestiona retención | ✅ Sí | Guarda la configuración |
| Permite restaurar datos | ✅ Sí | No ejecuta restores |
| Se usa con Site Recovery | ❌ No directamente | ✅ Sí |

---

# 🧠 Cómo funciona en la práctica

1. Creas un **Recovery Services Vault**
2. Configuras una política de **Azure Backup**
3. Los backups se almacenan en el Vault

---

# 🎯 Regla mental AZ-305

Si lees:

- “Backup”
- “Retención 30 días”
- “Restore point”
- “Point-in-time restore”

👉 Azure Backup

Si lees:

- “Vault”
- “Contenedor”
- “Dónde se almacenan backups”

👉 Recovery Services Vault

---

# 🏁 En una frase

Azure Backup hace las copias.  
Recovery Services Vault las almacena.

