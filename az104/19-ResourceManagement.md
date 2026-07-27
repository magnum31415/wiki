[AZ104-INDEX](./readme.md)

# 19 - Azure Resource Management (AZ-104)

> Este documento resume la teoría de **Azure Resource Manager (ARM)** y la organización de recursos en Azure más preguntada en el examen **AZ-104**.

---

# Índice

- Azure Resource Manager
- Management Groups
- Subscriptions
- Resource Groups
- Resources
- Resource Providers
- Resource Locks
- Tags
- Azure Resource IDs
- Scope
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Resource Manager (ARM)?

**Azure Resource Manager (ARM)** es el plano de administración (**Management Plane**) de Azure.

Permite:

- Crear recursos.
- Modificar recursos.
- Eliminar recursos.
- Aplicar RBAC.
- Aplicar Azure Policy.
- Gestionar despliegues mediante ARM Templates o Bicep.

Todos los recursos de Azure se administran mediante ARM.

---

# 2. Jerarquía de Azure

La organización de recursos sigue esta jerarquía:

```
Tenant

↓

Management Group

↓

Subscription

↓

Resource Group

↓

Resource
```

Los permisos y políticas se heredan hacia abajo.

---

# 3. Management Groups

Los **Management Groups** permiten administrar varias Subscriptions de forma centralizada.

Se utilizan para aplicar:

- Azure Policy.
- Azure RBAC.
- Iniciativas.
- Gobernanza.

Son habituales en organizaciones grandes.

---

# 4. Subscriptions

Una **Subscription** es un límite administrativo y de facturación.

Dentro de una Subscription pueden existir:

- Resource Groups.
- Recursos.
- Roles RBAC.
- Azure Policy.

Una organización puede tener varias Subscriptions.

---

# 5. Resource Groups

Un **Resource Group** es un contenedor lógico de recursos.

Los recursos de un mismo Resource Group:

- Comparten ciclo de vida.
- Comparten permisos.
- Pueden administrarse conjuntamente.

Un recurso solo puede pertenecer a **un único Resource Group**.

---

# 6. Recursos

Los recursos son los elementos que realmente se crean en Azure.

Ejemplos:

- Virtual Machines
- Storage Accounts
- Virtual Networks
- Load Balancers
- SQL Databases

Cada recurso pertenece a:

- Una Subscription.
- Un Resource Group.

---

# 7. Resource Providers

Cada tipo de recurso pertenece a un **Resource Provider**.

Ejemplos:

| Resource Provider | Recursos |
|-------------------|----------|
| Microsoft.Compute | Virtual Machines |
| Microsoft.Storage | Storage Accounts |
| Microsoft.Network | Redes |
| Microsoft.Web | App Service |

Antes de crear un recurso, el Resource Provider debe estar registrado.

---

# 8. Resource ID

Cada recurso tiene un identificador único.

Ejemplo:

```text
/subscriptions/{subscription-id}
/resourceGroups/RG1
/providers/Microsoft.Storage
/storageAccounts/storage01
```

El **Resource ID** identifica de forma única un recurso dentro de Azure.

---

# 9. Resource Locks

Los **Resource Locks** protegen recursos frente a modificaciones o eliminaciones accidentales.

Existen dos tipos.

## CanNotDelete

Permite modificar el recurso, pero impide eliminarlo.

---

## ReadOnly

Impide:

- Modificar.
- Eliminar.

El recurso queda en modo solo lectura.

---

# 10. Tags

Los **Tags** son pares **Clave = Valor** que permiten clasificar recursos.

Ejemplo:

```
Environment = Production

Owner = IT

CostCenter = 1001
```

Se utilizan para:

- Organización.
- Automatización.
- Facturación.
- Azure Policy.

---

# 11. Herencia de Tags

Los **Tags no se heredan automáticamente** desde un Resource Group a sus recursos.

Si se desea esta funcionalidad debe utilizarse:

- Azure Policy (Append o Modify).

Esta es una pregunta clásica del examen.

---

# 12. Scope

Muchas operaciones utilizan un **Scope**.

Ejemplos:

- Azure RBAC.
- Azure Policy.
- Locks.

Los Scopes válidos son:

- Management Group.
- Subscription.
- Resource Group.
- Resource.

---

# 13. Collection Placeholder

En algunos escenarios aparece:

```text
/subscriptions/{id}/resourceGroups
```

Esto **no es un Scope válido**.

`resourceGroups` es únicamente un **collection placeholder**.

Debe especificarse un Resource Group concreto o utilizar el Scope de la Subscription.

---

# 14. Mover recursos

Muchos recursos pueden moverse entre:

- Resource Groups.
- Subscriptions.

Siempre que sean compatibles con la operación.

No todos los recursos admiten ser movidos.

---

# 15. Buenas prácticas

Microsoft recomienda:

- Organizar recursos mediante **Management Groups**.
- Separar entornos (Dev, Test, Prod) mediante **Subscriptions**.
- Agrupar recursos relacionados en un mismo **Resource Group**.
- Utilizar **Tags** para organización y costes.
- Proteger recursos críticos mediante **Resource Locks**.

---

# Preguntas trampa del AZ-104

✅ Un recurso solo puede pertenecer a **un Resource Group**.

✅ Los permisos RBAC y Azure Policy **se heredan** desde el Scope superior.

✅ Los **Tags no se heredan automáticamente**.

✅ Los **Resource Locks** tienen prioridad sobre los permisos RBAC.

✅ **CanNotDelete** permite modificar el recurso.

✅ **ReadOnly** impide modificar y eliminar.

✅ Un **Resource Provider** debe estar registrado antes de crear recursos de ese tipo.

✅ `/subscriptions/{id}/resourceGroups` **no es un Scope válido**, ya que `resourceGroups` es un **collection placeholder**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Resource Manager | Plano de administración |
| Management Group | Agrupa Subscriptions |
| Subscription | Límite administrativo y de facturación |
| Resource Group | Contenedor lógico |
| Resource | Recurso de Azure |
| Resource Provider | Microsoft.Compute, Storage... |
| Resource ID | Identificador único |
| CanNotDelete | Impide eliminar |
| ReadOnly | Solo lectura |
| Tags | Organización y costes |
| Scope | MG → Subscription → RG → Resource |
| Collection Placeholder | No es un Scope válido |
| Resource Locks | Tienen prioridad sobre RBAC |
