[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# Comparativa RBAC: Virtual Machine Contributor vs Disk Snapshot Contributor

| Acción / Permiso | Virtual Machine Contributor | Disk Snapshot Contributor |
|---|---|---|
| Crear máquinas virtuales | ✅ | ❌ |
| Eliminar máquinas virtuales | ✅ | ❌ |
| Iniciar / detener VM | ✅ | ❌ |
| Reiniciar VM | ✅ | ❌ |
| Redimensionar VM | ✅ | ❌ |
| Administrar configuración VM | ✅ | ❌ |
| Attach / Detach disks | ✅ | ❌ |
| Leer configuración de discos asociados | ✅ | ❌ |
| Crear managed disks | ✅ Parcialmente (en contexto VM) | ❌ |
| Eliminar managed disks | ✅ Parcialmente (en contexto VM) | ❌ |
| Administrar NICs relacionadas con la VM | ✅ | ❌ |
| Crear snapshots | ❌ | ✅ |
| Eliminar snapshots | ❌ | ✅ |
| Leer snapshots | ❌ | ✅ |
| Restaurar snapshots | ❌ | ✅ |
| Exportar snapshots | ❌ | ✅ |
| Administrar recursos `Microsoft.Compute/snapshots` | ❌ | ✅ |
| Administrar recursos `Microsoft.Compute/virtualMachines` | ✅ | ❌ |
| Login RDP/SSH | ❌ | ❌ |
| Gestionar RBAC | ❌ | ❌ |
| Crear role assignments | ❌ | ❌ |

---

# Recursos Azure afectados

| Rol | Recursos principales |
|---|---|
| Virtual Machine Contributor | `Microsoft.Compute/virtualMachines` |
| Disk Snapshot Contributor | `Microsoft.Compute/snapshots` |

---

# Diferencia clave para AZ-104

| Concepto | Explicación |
|---|---|
| VM Contributor | Administra VMs y discos asociados a la VM |
| Disk Snapshot Contributor | Administra snapshots como recursos independientes |
| Snapshot | Tiene RBAC separado de VM y Managed Disk |

---

# Trampa típica de examen

Muchos candidatos creen:

```text
Virtual Machine Contributor = control total sobre discos y snapshots
```

❌ Incorrecto.

Los snapshots requieren permisos específicos.

---

# Regla rápida para memorizar

| Necesidad | Rol mínimo recomendado |
|---|---|
| Administrar VMs | Virtual Machine Contributor |
| Administrar snapshots | Disk Snapshot Contributor |
| Login VM | Virtual Machine User Login |
| Login administrador VM | Virtual Machine Administrator Login |

---

# Frase clave AZ-104

```text
Snapshots are independent Azure resources and require separate RBAC permissions.
```
