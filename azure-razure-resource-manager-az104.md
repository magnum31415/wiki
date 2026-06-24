[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

- [Azure Resource Locks (AZ-104)](#azure-resource-locks-az-104)
- [Herencia de Locks](#herencia-de-locks)
- [Relación con RBAC](#relación-con-rbac)
- [Locks y Movimiento de Recursos](#locks-y-movimiento-de-recursos)
- [Locks y Operaciones ARM](#locks-y-operaciones-arm)
- [Preguntas frecuentes de examen](#preguntas-frecuentes-de-examen)
- [Regla rápida para el examen](#regla-rápida-para-el-examen)

---

# Azure Resource Locks (AZ-104)

## ¿Qué son los Azure Resource Locks?

Los **Azure Resource Locks** son mecanismos de protección que evitan cambios accidentales en recursos de Azure.

Se pueden aplicar a:

- Suscripciones
- Resource Groups
- Recursos individuales

Los locks forman parte de **Azure Resource Manager (ARM)**.

Su objetivo principal es proteger recursos críticos frente a errores humanos.

---

## Tipos de Locks

Azure soporta dos tipos de locks:

### Delete (CanNotDelete)

Impide eliminar el recurso.

Permite:

- Leer el recurso
- Modificar el recurso

No permite:

- Eliminar el recurso

Ejemplo:

Un Storage Account con un lock Delete:

```text
Storage Account
└── Lock: Delete
```

Puedes:

- Crear contenedores
- Modificar configuración
- Cambiar networking

No puedes:

- Eliminar el Storage Account
```


### Read-only

Impide cualquier modificación.

Permite:

- Leer el recurso

No permite:

- Modificar
- Eliminar

Ejemplo:

```text
Virtual Network
└── Lock: Read-only
```

Puedes:

- Ver configuración

No puedes:

- Añadir subnets
- Modificar NSGs
- Eliminar la VNet
```


## Resumen rápido

| Lock | Leer | Modificar | Eliminar |
|--------|--------|------------|-----------|
| None | ✅ | ✅ | ✅ |
| Delete | ✅ | ✅ | ❌ |
| Read-only | ✅ | ❌ | ❌ |

---

# Herencia de Locks

Los locks se heredan.

Si aplicas un lock a un Resource Group:

```text
RG1
└── Lock: Delete
     │
     ├── VM1
     ├── Storage1
     └── VNET1
```

Todos los recursos del Resource Group heredan el lock.


## Prioridad de los Locks

Azure aplica siempre el lock más restrictivo.

Ejemplo:

```text
RG1
└── Delete

VM1
└── Read-only
```

Resultado:

```text
VM1 => Read-only
```

Porque Read-only es más restrictivo que Delete.

---

# Relación con RBAC

RBAC y Locks son mecanismos distintos.

### RBAC

Controla:

```text
¿Quién puede hacer algo?
```

Ejemplo:

```text
Usuario = Contributor
```

Puede modificar recursos.

---

### Locks

Controlan:

```text
¿Qué operaciones están prohibidas?
```

Ejemplo:

```text
Usuario = Owner
Lock = Read-only
```

Aunque sea Owner:

❌ No podrá modificar el recurso.



## ¿Puede un Owner eliminar un recurso con lock?

No.

Ni siquiera un Owner.

Primero debe eliminar el lock.

Ejemplo:

```text
VM1
└── Delete Lock
```

Para borrar la VM:

1. Eliminar el lock.
2. Eliminar la VM.

---

# Locks y Movimiento de Recursos

Pregunta clásica del AZ-104.

Los locks:

- No bloquean mover recursos entre Resource Groups.
- No bloquean mover recursos entre suscripciones (si el recurso soporta el movimiento).

Ejemplo:

```text
Storage1
└── Delete Lock
```

Puede moverse:

```text
RG1
 ↓
RG2
```

aunque tenga lock.

---

# Locks y Operaciones ARM

Los locks actúan sobre operaciones de Azure Resource Manager.

Ejemplo:

### Delete Lock

Bloquea:

```text
DELETE
```

### Read-only Lock

Bloquea:

```text
PUT
POST
PATCH
DELETE
```

Permite:

```text
GET
```

---

## Casos típicos AZ-104

### Proteger un Production Resource Group

```text
RG-PROD
└── Delete Lock
```

Evita borrados accidentales.

---

### Proteger una VNet crítica

```text
VNET-PROD
└── Read-only Lock
```

Evita cambios de configuración.

---

### Proteger un Recovery Services Vault

```text
BackupVault
└── Delete Lock
```

Evita eliminar backups accidentalmente.

---

# Preguntas frecuentes de examen

## ¿Un Delete Lock impide modificar un recurso?

No.

```text
Delete Lock = solo bloquea eliminar.
```

---

## ¿Un Read-only Lock impide eliminar?

Sí.

```text
Read-only = no modificar y no eliminar.
```

---

## ¿Los Locks se heredan?

Sí.

Desde:

- Subscription
- Resource Group

hacia los recursos contenidos.

---

## ¿Los Locks impiden mover recursos?

No.

Los recursos pueden moverse entre Resource Groups aunque tengan:

- Delete Lock
- Read-only Lock

---

# Regla rápida para el examen

Delete Lock:

```text
Puede modificarse.
No puede borrarse.
Puede moverse.
```

Read-only Lock:

```text
No puede modificarse.
No puede borrarse.
Puede moverse.
```

Herencia:

```text
Los locks aplicados a una Subscription o Resource Group
se heredan a todos los recursos hijos.
```
