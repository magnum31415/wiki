 Azure Hybrid Benefit (AHB)

## 🔎 ¿Qué es?

**Azure Hybrid Benefit** es un beneficio de licenciamiento que permite usar tus **licencias on-premises existentes** de:

- Windows Server
- SQL Server

para pagar **menos** cuando ejecutas cargas en Azure.

👉 Básicamente: *reutilizas tus licencias y reduces el coste de Azure.*

---

## 🎯 ¿Qué problema resuelve?

Si ya has comprado:

- SQL Server con Software Assurance
- Windows Server con Software Assurance

No necesitas volver a pagar la licencia incluida en el precio de la VM o Azure SQL.

Solo pagas el **coste de infraestructura (compute)**.

---

# 🧩 Dónde se puede aplicar

## 🔹 1️⃣ Azure Virtual Machines

- Windows Server
- SQL Server en VM

Sin AHB:
- Pagas VM + licencia Windows/SQL

Con AHB:
- Pagas solo la VM
- Usas tu licencia propia

---

## 🔹 2️⃣ Azure SQL (modelo vCore)

- Azure SQL Database
- Azure SQL Managed Instance

Solo disponible en **modelo vCore**
❌ No disponible en modelo DTU

---

# 📊 Impacto en coste

| Servicio | Ahorro aproximado |
|----------|-------------------|
| Windows Server VM | Hasta ~40% |
| SQL Server en VM | Hasta ~55% |
| Azure SQL (vCore) | Hasta ~30-40% |

---

# 🧠 Requisitos

- Tener licencias con **Software Assurance**
- Cumplir reglas de movilidad de licencias
- Declarar el uso en Azure

---

# 🎯 Regla mental AZ-305

Si lees:

- “Use existing SQL Server licenses”
- “Minimize cost using existing licenses”
- “Cost optimization with existing on-prem licenses”

👉 Respuesta: **Azure Hybrid Benefit**

---

# 🏁 En una frase

Azure Hybrid Benefit permite reutilizar licencias on-premises para pagar menos por recursos en Azure.
