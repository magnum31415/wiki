[AZ104-INDEX](./readme.md)

# 12 - Azure Policy (AZ-104)

> Este documento resume la teoría de **Azure Policy** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure Policy?
- Policy vs RBAC
- Scope
- Policy Definitions
- Initiatives
- Assignment
- Exemptions
- Policy Effects
- Compliance
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Policy?

**Azure Policy** permite **garantizar el cumplimiento (compliance)** de las normas de la organización.

Puede:

- Auditar recursos.
- Bloquear despliegues.
- Agregar Tags.
- Modificar configuraciones.
- Desplegar recursos automáticamente.

No controla **quién** puede hacer una acción, sino **qué está permitido**.

---

# 2. Azure Policy vs Azure RBAC

| Azure Policy | Azure RBAC |
|---------------|------------|
| Controla recursos | Controla permisos |
| Define qué está permitido | Define quién puede hacerlo |
| Compliance | Autorización |

Ambos servicios son complementarios.

---

# 3. Scope

Una Policy puede asignarse a:

- Management Group
- Subscription
- Resource Group

Los recursos heredan automáticamente las políticas del ámbito superior.

---

# 4. Policy Definition

Una **Policy Definition** contiene la regla que Azure debe evaluar.

Ejemplos:

- Solo permitir determinadas regiones.
- Obligar a utilizar Tags.
- Prohibir Public IPs.
- Obligar a utilizar HTTPS.

La definición por sí sola **no tiene efecto** hasta que se asigna.

---

# 5. Initiative

Una **Policy Initiative** (Policy Set) agrupa varias Policies bajo una única asignación.

Ejemplo:

```
Security Initiative

↓

Allowed Locations

↓

Required Tags

↓

HTTPS Only

↓

Allowed VM Sizes
```

Permite administrar varias Policies como un único conjunto.

---

# 6. Policy Assignment

Una **Policy Assignment** aplica una Policy o Initiative a un determinado Scope.

Sin una Assignment, la Policy no se evalúa.

---

# 7. Exemptions

Una **Policy Exemption** excluye recursos concretos del cumplimiento de una Policy.

Se utiliza cuando un recurso necesita quedar temporal o permanentemente fuera del control de la política.

---

# 8. Policy Effects

Los efectos más importantes son:

| Effect | Función |
|---------|---------|
| **Deny** | Bloquea el despliegue |
| **Audit** | Registra incumplimientos |
| **Append** | Agrega propiedades |
| **Modify** | Modifica el recurso |
| **DeployIfNotExists** | Despliega recursos automáticamente |
| **AuditIfNotExists** | Audita si falta un recurso |
| **Disabled** | Desactiva la Policy |

---

# 9. Deny

**Deny** impide crear o modificar recursos que incumplen la política.

Ejemplos:

- Regiones no permitidas.
- Tipos de recurso prohibidos.
- Public IPs.

El despliegue falla inmediatamente.

---

# 10. Audit

**Audit** permite crear el recurso, pero registra el incumplimiento.

Se utiliza para evaluar el impacto antes de aplicar una política **Deny**.

---

# 11. Append

**Append** agrega automáticamente propiedades que no existan.

Ejemplo típico:

Agregar un **Tag** obligatorio.

Solo afecta a **recursos nuevos o modificados**.

No modifica recursos existentes.

---

# 12. Modify

**Modify** cambia automáticamente determinadas propiedades durante el despliegue.

Ejemplo:

Agregar Tags.

A diferencia de **Append**, puede modificar propiedades existentes cuando la Policy lo permite.

---

# 13. DeployIfNotExists

Si un recurso necesario no existe, Azure lo despliega automáticamente.

Ejemplos habituales:

- Diagnostic Settings.
- Azure Monitor Agent.
- NSG Flow Logs.

Muy utilizado en entornos corporativos.

---

# 14. AuditIfNotExists

Comprueba si existe un recurso relacionado.

Si no existe:

↓

Marca el recurso como **No Compliant**.

No despliega automáticamente nada.

---

# 15. Allowed Locations

Una de las Policies más utilizadas.

Permite restringir el despliegue únicamente a determinadas regiones.

Ejemplo:

```
Germany West Central

Sweden Central
```

Si se intenta desplegar un recurso en otra región:

↓

Azure devuelve un error.

---

# 16. Required Tags

Permite obligar a que todos los recursos tengan determinados Tags.

Ejemplo:

- Owner
- Environment
- CostCenter
- Application

Muy utilizada en Landing Zones.

---

# 17. Compliance

Azure Policy evalúa continuamente los recursos.

Cada recurso tendrá uno de estos estados:

- Compliant
- Non-compliant

Esto permite conocer el nivel de cumplimiento de la organización.

---

# 18. Resource Providers

Una Policy también puede impedir el despliegue de determinados **Resource Types**.

Ejemplo:

```
Microsoft.Compute/virtualMachines
```

Mientras exista una Policy **Deny**, no será posible crear máquinas virtuales.

---

# 19. Buenas prácticas

Microsoft recomienda:

- Asignar Policies a nivel de **Management Group** cuando sea posible.
- Agrupar Policies mediante **Initiatives**.
- Comenzar con **Audit** antes de utilizar **Deny**.
- Utilizar **DeployIfNotExists** para automatizar configuraciones.

---

# Preguntas trampa del AZ-104

✅ **Azure Policy** controla **qué puede desplegarse**, no quién puede hacerlo.

✅ Una **Policy Definition** no tiene efecto hasta crear una **Policy Assignment**.

✅ Una **Initiative** agrupa varias Policies.

✅ **Append** agrega propiedades a recursos nuevos, pero **no modifica recursos existentes**.

✅ **DeployIfNotExists** despliega automáticamente recursos que faltan.

✅ **AuditIfNotExists** solo informa del incumplimiento.

✅ Una Policy **Deny** bloquea el despliegue inmediatamente.

✅ Las Policies asignadas a una **Subscription** se heredan por todos los Resource Groups y recursos.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Policy | Compliance |
| RBAC | Permisos |
| Scope | MG → Subscription → RG |
| Policy Definition | Regla |
| Initiative | Conjunto de Policies |
| Assignment | Aplica la Policy |
| Exemption | Exclusión |
| Deny | Bloquea |
| Audit | Solo registra |
| Append | Agrega propiedades |
| Modify | Modifica propiedades |
| DeployIfNotExists | Despliega automáticamente |
| AuditIfNotExists | Solo audita |
| Allowed Locations | Restringe regiones |
| Required Tags | Obliga Tags |
| Compliance | Compliant / Non-compliant |
