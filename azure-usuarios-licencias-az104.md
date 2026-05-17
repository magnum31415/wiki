# Microsoft Entra ID - Usuarios, Grupos y Licencias (AZ-104)

# Conceptos IMPORTANTES examen

| Concepto | Importancia |
|---|---|
| Licencias directas vs grupales | Muy alta |
| Dynamic Groups | Muy alta |
| Bulk Import CSV | Alta |
| Usuarios sincronizados | Alta |
| Soft Delete usuarios | Alta |
| Group-based licensing | Muy alta |

---

# 1. Licencias directas vs grupales

| Tipo | Cómo funciona |
|---|---|
| Direct license assignment | Licencia asignada directamente al usuario |
| Group-based licensing | Licencia heredada desde grupo |

---

# Ejemplo

## Directa

```text
User1
 └── Microsoft 365 E5
```

---

## Por grupo

```text
Group-M365-E5
 └── User1
```

↓

User1 hereda licencia automáticamente.

---

# Concepto clave examen

```text
Group-based licensing reduces administrative effort.
```

---

# 2. Usuarios pueden borrarse aunque tengan licencias

MUY importante.

---

# Correcto

```text
Users CAN be deleted
even if licenses are assigned.
```

---

# Qué ocurre al borrar usuario

Las licencias:

```text
se liberan automáticamente
```

↓

pueden reutilizarse.

---

# Esto aplica a

| Tipo licencia | ¿Usuario puede borrarse? |
|---|---|
| Directa | ✅ |
| Por grupo | ✅ |

---

# Concepto importante

Eliminar usuario:

```text
NO requiere quitar licencias primero
```

---

# 3. Grupos con licencias NO pueden borrarse

MUY típica pregunta examen.

---

# Incorrecto intentar borrar

```text
grupo con licencias asignadas
```

↓

Azure bloquea el borrado.

---

# Primero debes

```text
quitar las licencias del grupo
```

y luego:

```text
borrar grupo
```

---

# Regla rápida

| Objeto | ¿Se puede borrar con licencias? |
|---|---|
| Usuario | ✅ Sí |
| Grupo | ❌ No |

---

# 4. Dynamic Groups

## Qué son

Grupos automáticos basados en reglas.

---

# Ejemplo

```text
(user.department -eq "IT")
```

↓

usuarios IT entran automáticamente.

---

# Tipos importantes

| Tipo grupo | Explicación |
|---|---|
| Assigned | Manual |
| Dynamic User | Automático usuarios |
| Dynamic Device | Automático dispositivos |

---

# 5. Bulk Import Users

Formato soportado:

```text
CSV
```

NO:

- XML
- JSON
- ARM templates

---

# Ejemplo CSV

```csv
User name,Department
user1@contoso.com,IT
```

---

# 6. Soft Delete usuarios

Cuando borras un usuario:

```text
NO desaparece inmediatamente
```

↓

entra en:

```text
Deleted Users
```

durante:

```text
30 días
```

---

# Durante ese tiempo

Puedes:

- restaurar usuario
- recuperar grupos
- recuperar memberships

---

# 7. Usuarios sincronizados (Hybrid Identity)

Con Entra Connect:

```text
usuarios on-prem sincronizados
```

---

# Importante examen

| Tipo usuario | Dónde se administra |
|---|---|
| Cloud-only | Entra ID |
| Synced user | AD on-prem |

---

# Trampa típica

Intentar modificar atributos cloud de:

```text
usuario sincronizado
```

↓

muchos atributos son:

```text
read-only en cloud
```

---

# 8. Group-Based Licensing

MUY importante AZ-104.

---

# Qué hace

Permite asignar:

```text
licencias a grupos
```

↓

usuarios heredan automáticamente.

---

# Beneficios

| Beneficio | Explicación |
|---|---|
| Automatización | Menos administración |
| Escalabilidad | Fácil onboarding |
| Consistencia | Todos mismos servicios |

---

# Flujo típico

```text
Usuario entra grupo
       ↓
Licencia heredada
       ↓
Servicios habilitados
```

---

# 9. Licenses inheritance

| Escenario | Resultado |
|---|---|
| Usuario sale grupo | Pierde licencia heredada |
| Usuario entra grupo | Recibe licencia |
| Usuario borrado | Licencia liberada |

---

# 10. Nested groups

MUY importante.

---

# Group-based licensing

```text
NO soporta nested groups
```

---

# Ejemplo

```text
GroupA
 └── GroupB
      └── User1
```

↓

User1:

```text
NO hereda licencia
```

desde GroupA.

---

# 11. Licencias y Dynamic Groups

Combinación MUY usada.

---

# Ejemplo enterprise

```text
Dynamic Group:
department = IT
```

↓

grupo recibe:

```text
M365 E5
```

↓

todos los IT reciben licencia automáticamente.

---

# 12. Roles importantes relacionados

| Rol | Función |
|---|---|
| Global Administrator | Control total |
| License Administrator | Gestionar licencias |
| User Administrator | Gestionar usuarios |
| Groups Administrator | Gestionar grupos |

---

# Trampas típicas AZ-104

## Trampa 1

```text
Hay que quitar licencias antes de borrar usuarios
```

❌ Incorrecto.

---

# Trampa 2

```text
Se puede borrar grupo con licencias asignadas
```

❌ Incorrecto.

---

# Trampa 3

```text
Nested groups funcionan con licensing
```

❌ Incorrecto.

---

# Trampa 4

```text
Bulk import usa XML
```

❌ Incorrecto.

---

# Conceptos MUY preguntados

| Tema | Frecuencia |
|---|---|
| Dynamic Groups | Muy alta |
| Group-based licensing | Muy alta |
| User synchronization | Muy alta |
| CSV bulk import | Alta |
| Soft delete | Alta |

---

# Reglas rápidas examen

```text
Users can be deleted even if licenses are assigned.
```

```text
Groups with assigned licenses cannot be deleted.
```

```text
Group-based licensing does not support nested groups.
```

```text
CSV is the supported format for bulk user import.
```

```text
Dynamic Groups minimize administrative effort.
```
