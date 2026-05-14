[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


# Azure VM Redeploy y Scheduled Maintenance (AZ-104)

## Scheduled Maintenance

Azure realiza mantenimiento periódico sobre la infraestructura física.

Esto puede afectar:

- Hosts físicos
- Hypervisors
- Networking
- Hardware Azure

---

## Qué ocurre durante el mantenimiento

Azure puede:

- Reiniciar hosts
- Migrar workloads
- Reiniciar VMs
- Aplicar actualizaciones plataforma

---

## Maintenance Event

Una VM puede recibir alertas como:

```text
Scheduled platform maintenance
```

---

## Objetivo típico examen

AZ-104 suele preguntar cómo:

- Evitar impacto
- Mover VM a otro host
- Aplicar mantenimiento manualmente
- Resolver problemas host físicos

---

## Concepto clave

### Host físico

Una Azure VM corre sobre:

```text
Host físico Azure
```

---

## Importante

La VM NO existe directamente “en Azure”.

Existe sobre:

- Hardware físico
- Hypervisor Azure

---

# Tabla resumen

| Acción | Qué hace | Reinicia VM | Cambia host físico | Mantiene discos | Puede perder temporary disk | Uso típico | ¿Permite mover VM a otra subscription? | ¿Permite mover VM a otro Resource Group? |
|---|---|---|---|---|---|---|---|---|
| Redeploy | Mueve la VM a otro host Azure | ✅ | ✅ | ✅ | ✅ | Problemas host físico, scheduled maintenance | ❌ | ❌ |
| Restart | Reinicia sistema operativo/VM | ✅ | ❌ | ✅ | ❌ normalmente | Reinicio normal | ❌ | ❌ |
| Reapply | Reaplica configuración VM al host actual | A veces | ❌ | ✅ | ❌ | Problemas configuración/extensiones | ❌ | ❌ |
| One-time Update | Aplica mantenimiento manualmente | Puede | ❌ | ✅ | ❌ normalmente | Adelantar maintenance updates | ❌ | ❌ |
| Enable (Updates blade) | Habilita Update Management / patch orchestration | ❌ normalmente | ❌ | ✅ | ❌ | Gestión automática updates | ❌ | ❌ |
| Move to another subscription | Mueve recursos Azure entre subscriptions | ❌ normalmente | ❌ | ✅ | ❌ | Reorganización tenant/subscriptions | ✅ | ✅ normalmente |
| Move to another Resource Group | Mueve la VM a otro RG dentro de la subscription | ❌ normalmente | ❌ | ✅ | ❌ | Reorganización recursos | ❌ | ✅ |

![azure-vm-redeploy-reapply](./img/azure/redeploy-reapply.jpg)

---

## Diferencia importante

| Acción | Cambia host Azure | Configura updates | Cambia Resource Group | Cambia Subscription |
|---|---|---|---|---|
| Redeploy | ✅ | ❌ | ❌ | ❌ |
| One-time Update | ❌ | ✅ | ❌ | ❌ |
| Enable (Updates blade) | ❌ | ✅ | ❌ | ❌ |
| Move Resource Group | ❌ | ❌ | ✅ | ❌ |
| Move Subscription | ❌ | ❌ | ✅ normalmente | ✅ |

---

# Qué hace "Enable" en Updates blade

Al pulsar:

```text
Enable
```

Azure normalmente:

- Habilita Update Management
- Configura patch orchestration
- Permite gestión updates

Pero:

❌ NO mueve la VM  
❌ NO cambia host físico  
❌ NO hace redeploy  

---

# Reglas rápidas AZ-104

```text
Enable in the Updates blade configures update management, not VM relocation.
```

```text
Only Redeploy moves the VM to another Azure host.
```


## Diferencia clave AZ-104

| Acción | Palabra clave examen |
|---|---|
| Redeploy | Move VM to another host |
| Restart | Reboot VM |
| Reapply | Reapply configuration |
| One-time Update | Apply maintenance |






---

# Diferencia clave AZ-104

| Acción | Palabra clave examen |
|---|---|
| Redeploy | Move VM to another host |
| Restart | Reboot VM |
| Reapply | Reapply configuration |
| One-time Update | Apply maintenance |

---

# Regla rápida examen

```text
Only Redeploy guarantees moving the VM to another Azure host.
```

---

## Redeploy VM

### Qué hace

```text
Redeploy
```

mueve la VM a otro host físico Azure.

---

## Qué ocurre durante Redeploy

Azure:

1. Apaga la VM
2. La mueve a otro host físico
3. Recrea el estado computación
4. Mantiene discos y configuración

---

### Qué cambia

| Elemento | Cambia |
|---|---|
| Host físico | ✅ |
| Compute host | ✅ |
| Temporary disk | Puede perderse |
| VM runtime | Reinicia |

---

### Qué NO cambia

| Elemento | Se mantiene |
|---|---|
| Managed disks | ✅ |
| NICs | ✅ |
| IPs privadas | ✅ normalmente |
| Configuración VM | ✅ |
| OS disk | ✅ |

---

## Cuándo usar Redeploy

| Escenario | Usar Redeploy |
|---|---|
| Host con problemas | ✅ |
| Scheduled maintenance | ✅ |
| VM stuck | ✅ |
| Problemas networking host | ✅ |
| Performance issues host | ✅ |

---

## Qué NO es Redeploy

Redeploy NO es:

- Restart
- Reapply
- One-time update

---

## Restart VM

### Qué hace

```text
Restart
```

reinicia la VM.

---

### Qué NO hace

❌ NO cambia host físico.

---

## Reapply VM

### Qué hace

Reaplica configuración VM al host actual.

---

### Qué NO hace

❌ NO mueve la VM.

---

## One-time Update

### Qué es

Permite aplicar mantenimiento manualmente antes del maintenance window.

---

### Qué hace

✅ Aplicar updates plataforma  
✅ Adelantar mantenimiento  

---

### Qué NO hace

❌ NO mueve VM a otro host  
❌ NO garantiza relocation  

---

## Diferencia importante

| Acción | Cambia host físico |
|---|---|
| Restart | ❌ |
| Reapply | ❌ |
| One-time update | ❌ |
| Redeploy | ✅ |

---

## Trampa típica AZ-104

Muchos candidatos ven:

```text
maintenance
```

y piensan:

```text
update
```

❌ Incorrecto.

---

## La palabra clave examen

```text
move VM to another host
```

o

```text
relocate
```

---

## Eso implica

✅ Redeploy

---

## Temporary Disk

## Importante

Durante Redeploy:

```text
Temporary disk puede perderse
```

---

## Ejemplo típico

Windows:

```text
D:
```

Linux:

```text
/mnt
```

---

## Managed Disks

### Importante

Managed Disks:

✅ sobreviven Redeploy

---

## Maintenance Control

Azure permite:

- Planned maintenance notifications
- Maintenance configurations
- Self-service maintenance

---

## Qué quiere evaluar el examen

| Concepto | Importancia |
|---|---|
| Redeploy VM | Muy alta |
| Scheduled maintenance | Alta |
| Host relocation | Muy alta |
| Temporary disk | Alta |
| Restart vs Redeploy | Muy alta |

---

## Tabla resumen examen

| Acción | Reinicia VM | Cambia host |
|---|---|---|
| Restart | ✅ | ❌ |
| Reapply | A veces | ❌ |
| One-time update | Puede | ❌ |
| Redeploy | ✅ | ✅ |

---

## Regla rápida examen

```text
Redeploy relocates a VM to a new Azure host.
```

---

## Frases clave AZ-104

```text
Redeploy moves a VM to another physical Azure host.
```

```text
One-time update applies maintenance but does not relocate the VM.
```

```text
Temporary disks may be lost during redeploy.
```
