[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 🔐 Comparativa Completa: Seguridad y Control en Azure

---

# 1️⃣ Azure RBAC (IAM)

📍 Qué es:
Sistema de autorización basado en roles para recursos de Azure.

📍 Controla:
El plano de control (Control Plane)

📍 Ejemplos:
- Crear una VM
- Borrar una VNet
- Asignar permisos

📍 Fórmula:
Security Principal + Role + Scope

📍 Portal:
Recurso → Access Control (IAM)

---

# 2️⃣ Microsoft Entra ID Roles (antes Azure AD roles)

📍 Qué es:
Roles administrativos del tenant de identidad.

📍 Controla:
Identidades, usuarios, grupos, aplicaciones.

📍 Ejemplos:
- Global Administrator
- Application Administrator
- Security Administrator

📍 Actúa sobre:
El directorio (no sobre recursos como VMs o VNets).

📍 Diferencia clave:
Entra roles ≠ permisos sobre recursos Azure.

---

# 3️⃣ Data Plane RBAC

📍 Qué es:
Permisos sobre los datos dentro de un recurso.

📍 Ejemplos:
- Leer blobs en una Storage Account
- Ejecutar queries en SQL
- Acceder a secretos en Key Vault

📍 Importante:
Puedes tener Reader (control plane)
y no poder leer los datos (data plane).

---

# 4️⃣ Azure Policy

📍 Qué es:
Sistema de gobernanza y cumplimiento.

📍 Controla:
Qué se puede o no desplegar.

📍 Ejemplos:
- Solo permitir regiones EU
- Obligar a usar tags
- Bloquear IP pública

📍 No da permisos.
📍 No asigna roles.

RBAC dice quién puede.
Policy dice si está permitido.

---

# 5️⃣ Resource Locks

📍 Qué es:
Mecanismo para bloquear cambios accidentales.

Tipos:
- CanNotDelete
- ReadOnly

📍 Ejemplo:
Evitar que alguien borre un RG crítico.

📍 Importante:
Lock puede bloquear incluso a un Owner.

---

# 6️⃣ Azure Access Policies

⚠️ Se usan principalmente en Azure Key Vault (modelo antiguo).

📍 Qué es:
Modelo de permisos específico para acceso a secretos, claves y certificados.

📍 Sustituido en la práctica por:
Azure RBAC for Key Vault (modelo recomendado).

📍 Diferencia:
Access Policy es específica del recurso.
RBAC es modelo unificado.

---

# 📊 Comparativa Visual

| Controla | Azure RBAC | Entra Roles | Data Plane RBAC | Policy | Locks | Access Policy |
|------------|-------------|-------------|----------------|--------|--------|---------------|
| Recursos Azure | ✅ | ❌ | ❌ | ✅ (reglas) | ✅ | ❌ |
| Identidades | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Datos dentro del recurso | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ (Key Vault) |
| Bloquea despliegues | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| Bloquea eliminación | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

---

# 🧠 Ejemplo completo práctico

Juan es Contributor en una Subscription.

✔ RBAC permite crear VM.

Pero:
Existe Policy que solo permite West Europe.

Si crea en East US:
❌ Bloqueado por Policy.

Además:
El RG tiene un Lock CanNotDelete.

Si intenta borrar:
❌ Bloqueado por Lock.

Y si intenta leer blobs:
Depende de Data Plane RBAC.

---

# 🎯 Regla mental examen

Permisos → RBAC  
Reglas → Policy  
Protección accidental → Locks  
Identidad → Entra ID Roles  
Acceso a datos → Data Plane RBAC  
Key Vault antiguo → Access Policies  

---

# 🔥 La clave para no confundirte

RBAC = Quién puede  
Policy = Qué está permitido  
Lock = Nadie lo borra  
Entra Roles = Admin del tenant  
Data Plane = Acceso a datos reales  

---

Si quieres, te hago ahora un diagrama jerárquico tipo mapa mental visual que te lo deja grabado para el examen.
