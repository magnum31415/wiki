[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

- [Azure RBAC Conditions (ABAC) para Azure Storage](#azure-rbac-conditions-abac-para-azure-storage)

  - [Introducción](#introducción)

- [Conceptos fundamentales](#conceptos-fundamentales)

- [Azure RBAC](#azure-rbac)
  - [¿Qué es?](#qué-es)

- [Autenticación vs Autorización](#autenticación-vs-autorización)
  - [Ejemplo](#ejemplo)

- [Scopes en Azure RBAC](#scopes-en-azure-rbac)

- [Herencia de permisos](#herencia-de-permisos)

- [Roles importantes en Azure Storage](#roles-importantes-en-azure-storage)

- [Reader](#reader)
  - [Qué permite](#qué-permite)
  - [Qué NO permite](#qué-no-permite)
  - [Importante para examen](#importante-para-examen)

- [Storage Blob Data Reader](#storage-blob-data-reader)
  - [Qué permite](#qué-permite-1)
  - [Scope habitual](#scope-habitual)

- [Storage Blob Data Owner](#storage-blob-data-owner)
  - [Qué permite](#qué-permite-2)

- [Azure RBAC Conditions](#azure-rbac-conditions)
  - [¿Qué son?](#qué-son)

- [Concepto clave](#concepto-clave)

- [Funcionamiento lógico](#funcionamiento-lógico)

- [Importante](#importante)

- [Azure ABAC (Attribute-Based Access Control)](#azure-abac-attribute-based-access-control)

- [Ejemplos de atributos](#ejemplos-de-atributos)
  - [Recurso](#recurso)
  - [Operación](#operación)
  - [Principal](#principal)

- [Ejemplo conceptual](#ejemplo-conceptual)
  - [Rol](#rol)
  - [Condición](#condición)
  - [Resultado](#resultado)

- [Operadores lógicos](#operadores-lógicos)

- [AND](#and)

- [OR](#or)

- [NOT](#not)

- [Ejemplo típico de examen](#ejemplo-típico-de-examen)
  - [Condición](#condición-1)

- [Interpretación correcta](#interpretación-correcta)

- [Caso 1 – Leer blob en cont1](#caso-1--leer-blob-en-cont1)
  - [Evaluación](#evaluación)

- [Caso 2 – Leer blob en cont2](#caso-2--leer-blob-en-cont2)
  - [Evaluación](#evaluación-1)

- [Caso 3 – Operación DELETE](#caso-3--operación-delete)
  - [Evaluación](#evaluación-2)

- [Evaluación mental correcta para examen](#evaluación-mental-correcta-para-examen)

- [Paso 1 – ¿Existe un rol que permita la acción?](#paso-1--existe-un-rol-que-permita-la-acción)

- [Paso 2 – ¿Existe condición?](#paso-2--existe-condición)

- [Paso 3 – ¿La condición devuelve TRUE?](#paso-3--la-condición-devuelve-true)

- [Scope y condiciones](#scope-y-condiciones)

- [Ejemplo](#ejemplo-1)

- [Relación entre RBAC y condiciones](#relación-entre-rbac-y-condiciones)

- [Casos típicos del examen AZ-104](#casos-típicos-del-examen-az-104)

- [Error típico 1](#error-típico-1)

- [Error típico 2](#error-típico-2)

- [Error típico 3](#error-típico-3)

- [Error típico 4](#error-típico-4)

- [Control Plane vs Data Plane](#control-plane-vs-data-plane)

- [Importante para examen](#importante-para-examen-1)

- [Reader](#reader-1)

- [Storage Blob Data Reader](#storage-blob-data-reader-1)

- [Resumen rápido](#resumen-rápido)

- [Ejemplo final resumido](#ejemplo-final-resumido)

- [Referencias oficiales](#referencias-oficiales)

  - [Azure RBAC Conditions](#azure-rbac-conditions-1)
  - [Azure RBAC Condition Format](#azure-rbac-condition-format)
  - [Built-in Roles](#built-in-roles)


# Azure RBAC Conditions (ABAC) para Azure Storage

## Introducción

Azure permite controlar el acceso a recursos mediante:

- RBAC (Role-Based Access Control)
- RBAC Conditions (condiciones)
- ABAC (Attribute-Based Access Control)

En Azure Storage, las condiciones RBAC permiten restringir el acceso a:

- Containers
- Blobs
- Operaciones concretas
- Paths
- Tags
- Atributos del recurso

---

# Conceptos fundamentales

---

# Azure RBAC

## ¿Qué es?

RBAC (Role-Based Access Control) es el sistema de autorización de Azure.

Define:

- Quién puede acceder
- Qué acciones puede realizar
- Sobre qué recursos

---

# Autenticación vs Autorización

| Concepto | Significado |
|---|---|
| Autenticación | Verificar identidad |
| Autorización | Verificar permisos |

## Ejemplo

### Microsoft Entra ID

Verifica:

```text
El usuario es válido
```

### RBAC

Verifica:

```text
El usuario puede leer blobs
```

---

# Scopes en Azure RBAC

Los roles pueden asignarse en diferentes niveles:

| Scope | Ejemplo |
|---|---|
| Management Group | Organización completa |
| Subscription | Todas las resources |
| Resource Group | Recursos concretos |
| Resource | Un recurso específico |

---

# Herencia de permisos

Los permisos asignados en scopes superiores se heredan.

## Ejemplo

Rol asignado en:

```text
Subscription
```

↓

aplica a:

- Resource Groups
- Storage Accounts
- Containers
- Blobs

---

# Roles importantes en Azure Storage

---

# Reader

## Qué permite

- Ver configuración
- Ver propiedades
- Ver recursos Azure

## Qué NO permite

NO permite acceder a datos del blob.

## Importante para examen

```text
Reader NO puede leer blobs
```

---

# Storage Blob Data Reader

## Qué permite

Permite:

- Leer blobs
- Descargar blobs
- Leer contenido blob

## Scope habitual

Puede asignarse a:

- Subscription
- Storage Account
- Container
- Blob

---

# Storage Blob Data Owner

## Qué permite

Control total sobre datos blob:

- Leer
- Escribir
- Eliminar
- Gestionar ACLs

---

# Azure RBAC Conditions

## ¿Qué son?

Son expresiones lógicas que restringen un rol RBAC.

Azure evalúa la condición antes de permitir el acceso.

---

# Concepto clave

RBAC define:

```text
QUÉ acciones están permitidas
```

La condición define:

```text
CUÁNDO o SOBRE QUÉ recursos se permiten
```

---

# Funcionamiento lógico

Azure permite el acceso únicamente cuando:

```text
Rol válido
AND
Condición == TRUE
```

---

# Importante

Aunque el usuario tenga el rol:

```text
Storage Blob Data Reader
```

si la condición devuelve:

```text
FALSE
```

el acceso se deniega.

---

# Azure ABAC (Attribute-Based Access Control)

Las condiciones RBAC son una forma de ABAC.

Azure usa atributos para evaluar permisos.

---

# Ejemplos de atributos

## Recurso

- Container name
- Blob name
- Blob path

## Operación

- Read
- Write
- Delete

## Principal

- Usuario
- Grupo

---

# Ejemplo conceptual

## Rol

```text
Storage Blob Data Reader
```

## Condición

```text
Solo containers llamados cont1
```

## Resultado

El usuario puede leer:

```text
cont1/*
```

pero NO:

```text
cont2/*
```

---

# Operadores lógicos

Azure RBAC Conditions usa lógica booleana.

---

# AND

Ambas condiciones deben ser TRUE.

```text
TRUE AND TRUE = TRUE
```

---

# OR

Solo una debe ser TRUE.

```text
TRUE OR FALSE = TRUE
```

---

# NOT

Invierte el resultado.

```text
NOT TRUE = FALSE
```

---

# Ejemplo típico de examen

## Condición

```text
requested action is NOT blob read
OR
container name = cont1
```

---

# Interpretación correcta

La condición devuelve TRUE cuando:

- la operación NO es lectura
OR
- el container es cont1

---

# Caso 1 – Leer blob en cont1

## Evaluación

### NOT blob read

```text
FALSE
```

porque sí es lectura.

### container = cont1

```text
TRUE
```

Resultado:

```text
FALSE OR TRUE = TRUE
```

Acceso permitido.

---

# Caso 2 – Leer blob en cont2

## Evaluación

### NOT blob read

```text
FALSE
```

### container = cont1

```text
FALSE
```

Resultado:

```text
FALSE OR FALSE = FALSE
```

Acceso denegado.

---

# Caso 3 – Operación DELETE

## Evaluación

### NOT blob read

```text
TRUE
```

porque DELETE no es READ.

Resultado:

```text
TRUE OR FALSE = TRUE
```

Acceso permitido.

---

# Evaluación mental correcta para examen

Siempre seguir este orden:

---

# Paso 1 – ¿Existe un rol que permita la acción?

Ejemplo:

```text
Storage Blob Data Reader
```

Sí permite leer blobs.

---

# Paso 2 – ¿Existe condición?

Si existe:

```text
Condition1
```

debe evaluarse.

---

# Paso 3 – ¿La condición devuelve TRUE?

## Si TRUE

→ acceso permitido

## Si FALSE

→ acceso denegado

---

# Scope y condiciones

Las condiciones también heredan según el scope.

---

# Ejemplo

## Rol asignado en Subscription

Afecta a:

- todos los storage accounts
- todos los containers
- todos los blobs

PERO:

la condición sigue filtrando.

---

# Relación entre RBAC y condiciones

| Elemento | Función |
|---|---|
| RBAC Role | Permiso base |
| Condition | Restricción |
| Scope | Dónde aplica |

---

# Casos típicos del examen AZ-104

---

# Error típico 1

Pensar:

```text
Reader puede leer blobs
```

Incorrecto.

---

# Error típico 2

Ignorar la condición RBAC.

---

# Error típico 3

Olvidar que:

```text
Condition debe devolver TRUE
```

---

# Error típico 4

Confundir control plane con data plane.

---

# Control Plane vs Data Plane

| Tipo | Ejemplo |
|---|---|
| Control Plane | Crear storage account |
| Data Plane | Leer blob |

---

# Importante para examen

## Reader

Solo control plane.

## Storage Blob Data Reader

Data plane.

---

# Resumen rápido

| Concepto | Idea clave |
|---|---|
| Reader | NO accede a blobs |
| Storage Blob Data Reader | Sí accede a blobs |
| RBAC Conditions | Restringen permisos |
| ABAC | Acceso basado en atributos |
| Condition == TRUE | Acceso permitido |
| Condition == FALSE | Acceso denegado |
| Scope | Define dónde aplica |

---

# Ejemplo final resumido

## Usuario

```text
Storage Blob Data Reader
```

## Condición

```text
Solo containers cont1
```

## Resultado

| Blob | Acceso |
|---|---|
| cont1/blob1 | ✅ |
| cont2/blob2 | ❌ |

---

# Referencias oficiales

## Azure RBAC Conditions

https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-overview

## Azure RBAC Condition Format

https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-format

## Built-in Roles

https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles
