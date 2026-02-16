[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# 🔐 Azure Key Vault – Tabla de Tiers y Opciones

Azure Key Vault tiene **dos tiers principales**:

- Standard
- Premium

---

# 📊 Comparativa de Tiers

| Característica | Key Vault Standard | Key Vault Premium | Azure Managed HSM |
|---------------|-------------------|-------------------|--------------------|
| Secretos (passwords, connection strings) | ✅ | ✅ | ❌ (solo claves) |
| Claves software (RSA, EC) | ✅ | ✅ | ❌ |
| Claves HSM | ❌ | ✅ | ✅ |
| HSM dedicado (single-tenant) | ❌ | ❌ | ✅ |
| Protección FIPS 140-2 Level 2 | ❌ | ✅ | ❌ |
| Protección FIPS 140-2 Level 3 | ❌ | ❌ | ✅ |
| Bring Your Own Key (BYOK) con HSM | ❌ | ✅ | ✅ |
| Security domain dedicado por cliente | ❌ | ❌ | ✅ |
| Integración con Azure Storage Encryption | ✅ | ✅ | ✅ |
| Integración con Azure Disk Encryption | ✅ | ✅ | ✅ |
| Acceso con RBAC | ✅ | ✅ | ✅ |
| Access Policies | ✅ | ✅ | ❌ (solo RBAC) |
| Soft Delete | ✅ (obligatorio) | ✅ | ✅ |
| Purge Protection | ✅ | ✅ | ✅ |
| Private Endpoint | ✅ | ✅ | ✅ |
| Alta disponibilidad entre zonas | ✅ | ✅ | ✅ (cluster multi-nodo) |
| Coste | Bajo | Más alto | Alto |


- **Standard** → secretos y claves software.
- **Premium** → claves HSM (multi-tenant, FIPS Level 2).
- **Managed HSM** → HSM dedicado, FIPS Level 3, aislamiento criptográfico real.

 **Regla mental rápida**
- ¿Solo secretos? → Standard  
- ¿HSM pero sin aislamiento físico dedicado? → Premium  
- ¿FIPS 140-2 Level 3 + security domain dedicado? → Azure Managed HSM

### Azure Key Vault – Disponibilidad y Resiliencia

| Característica                  | Standard | Premium |
| ------------------------------- | -------- | ------- |
| Alta disponibilidad regional    | ✅        | ✅       |
| Zone redundancy                 | ✅        | ✅       |
| Replicación a región secundaria | ✅        | ✅       |
| HSM (Hardware-backed keys)      | ❌        | ✅       |
| FIPS 140-2 Level 2              | ❌        | ✅       |




---

# 🧱 ¿Qué almacena Key Vault?

| Tipo | Descripción |
|------|-------------|
| Secrets | Passwords, connection strings, tokens |
| Keys | Claves criptográficas (RSA, EC) |
| Certificates | Certificados SSL/TLS (basados en claves) |

---

# 🔷 Diferencia clave: HSM

## 🔐 HSM (Hardware Security Module)

Disponible solo en **Premium**.

- Las claves nunca salen del módulo hardware.
- Cumple requisitos regulatorios estrictos.
- Ideal para banca, gobierno, compliance fuerte.

---

# 🧠 Cuándo usar cada tier

## ✅ Standard

- Guardar secretos
- Claves software
- Aplicaciones normales
- Integración con App Service / Functions
- Coste optimizado

## 🔒 Premium

- Requisitos regulatorios
- BYOK con HSM
- Protección criptográfica avanzada
- Firma digital segura
- Entornos financieros

---

# 🔷 Azure Managed HSM (Relacionado)

Servicio separado de Key Vault Premium.

| Característica | Managed HSM |
|---------------|-------------|
| Solo claves (no secretos) | ✅ |
| HSM dedicado | ✅ |
| Escalado horizontal | ✅ |
| Multi-tenant | ❌ (instancia dedicada) |
| Escenarios bancarios | ✅ |

---

# 🎯 Regla mental AZ-305

- “Store passwords / connection strings” → Standard
- “HSM required” → Premium
- “Compliance FIPS Level 2” → Premium
- “Dedicated HSM cluster” → Managed HSM

---

# 📌 Frase para memorizar

Standard = secretos y claves software.  
Premium = HSM y cumplimiento avanzado.


| Tipo          | Dónde se generó la clave  | Nivel seguridad |
| ------------- | ------------------------- | --------------- |
| Import simple | Generada fuera, importada | Software        |
| BYOK con HSM  | Generada en HSM externo   | HSM hardware    |


## Se pueden añadir claves propias a Azure Key Vault?

### ✅ 1️⃣ Crear claves dentro de Key Vault

Puedes:
- Crear claves RSA / EC directamente en Key Vault
- Pueden ser:
  - Software-protected (Standard)
  - HSM-protected (Premium)

👉 Aquí la clave nace dentro de Azure.

### ✅ 2️⃣ BYOK (Bring Your Own Key)

Sí, puedes importar tus propias claves.

- Escenarios:
  - Clave generada on-prem
  - Clave generada en un HSM externo
  - Clave de proveedor externo
 


Luego se importa a:
- Key Vault Standard (clave software)
- Key Vault Premium (clave HSM)
