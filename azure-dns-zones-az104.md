# Azure DNS Zones y Auto-Registration (AZ-104)

## Conceptos clave

En Azure existen dos tipos principales de zonas DNS:

| Tipo de zona | Uso | Accesible desde Internet | Soporta auto-registration |
|---|---|---|---|
| Public DNS Zone | Resolución pública de nombres | ✅ Sí | ❌ No |
| Private DNS Zone | Resolución interna dentro de VNets | ❌ No | ✅ Sí |

---

# Public DNS Zone

Una Public DNS Zone permite publicar registros DNS accesibles desde Internet.

Ejemplo:

```text
contoso.com
```

Permite crear registros como:

```text
www.contoso.com
api.contoso.com
mail.contoso.com
```

## Características

| Característica | Soportado |
|---|---|
| Resolución pública | ✅ |
| Uso desde Internet | ✅ |
| VNet Link | ❌ |
| Auto-registration | ❌ |
| Registro automático de VMs | ❌ |

---

# Private DNS Zone

Una Private DNS Zone permite resolución DNS privada dentro de Azure VNets.

Ejemplo:

```text
fabrikam.com
```

## Características

| Característica | Soportado |
|---|---|
| Resolución privada | ✅ |
| Visible desde Internet | ❌ |
| VNet Link | ✅ |
| Auto-registration | ✅ |
| Registro automático de VMs | ✅ |

---

# Virtual Network Link

Una Private DNS Zone debe estar vinculada a una VNet para que las VMs puedan usarla.

Ejemplo:

```text
fabrikam.com
→ linked to vnet1
```

## Qué permite

- Resolución DNS privada
- Registro automático de VMs (si auto-registration está habilitado)

---

# Auto-registration

## Qué es

Azure puede crear automáticamente registros DNS para las VMs conectadas a una VNet.

## Solo funciona con

✅ Private DNS Zones

## Nunca funciona con

❌ Public DNS Zones

---

# Qué registra Azure automáticamente

Azure registra:

- Nombre de la VM
- Dirección IP privada

Ejemplo:

```text
vm1.fabrikam.com -> 10.0.0.4
```

---

# Qué NO registra automáticamente

Azure NO registra:

- IPs públicas
- Registros en Public DNS Zones

Ejemplo NO válido:

```text
vm1.contoso.com -> 131.107.50.20
```

---

# Relación entre Auto-registration y VNet

Para que funcione auto-registration deben cumplirse TODAS estas condiciones:

| Requisito | Obligatorio |
|---|---|
| Private DNS Zone | ✅ |
| VNet Link | ✅ |
| Auto-registration enabled | ✅ |

---

# RBAC y DNS

Los roles RBAC NO crean registros DNS automáticamente.

Ejemplo:

```text
Owner
Contributor
DNS Zone Contributor
```

Solo permiten administrar recursos.

NO habilitan:

- auto-registration
- creación automática de registros
- sincronización automática


| Rol                  | Puede crear registros DNS | Puede borrar zona | Puede gestionar RBAC |
| -------------------- | ------------------------- | ----------------- | -------------------- |
| DNS Zone Contributor | ✅                         | ❌                 | ❌                    |
| Contributor          | ✅                         | ✅                 | ❌                    |
| Owner                | ✅                         | ✅                 | ✅                    |


---

# Diferencia entre IP privada y pública

## Azure Private DNS

Usa:

✅ Private IPs

Ejemplo:

```text
10.0.0.4
172.16.1.5
192.168.1.10
```

---

## Azure Public DNS

Los registros públicos deben crearse manualmente.

Ejemplo:

```text
www.contoso.com -> 131.107.50.20
```

---

# Trampas típicas del examen AZ-104

## Trampa 1

Pensar que Public DNS soporta auto-registration.

❌ Incorrecto

Solo Private DNS lo soporta.

---

## Trampa 2

Pensar que Azure registra automáticamente la Public IP.

❌ Incorrecto

Azure Private DNS registra únicamente la Private IP.

---

## Trampa 3

Pensar que RBAC crea registros DNS.

❌ Incorrecto

RBAC solo da permisos.

---

# Flujo correcto de auto-registration

```text
VM creada en VNet
        ↓
Private DNS Zone linked a la VNet
        ↓
Auto-registration enabled
        ↓
Azure crea automáticamente:
vm-name.private-zone -> private IP
```

---

# Ejemplo completo

## Configuración

```text
Private DNS Zone: fabrikam.com
VNet Link: vnet1
Auto-registration: Enabled
VM: vm1
Private IP: 10.0.0.4
```

## Resultado

Azure crea automáticamente:

```text
vm1.fabrikam.com -> 10.0.0.4
```

---

# Ejemplo incorrecto

## Configuración

```text
Public DNS Zone: contoso.com
VM public IP: 131.107.50.20
```

## Resultado

Azure NO crea automáticamente:

```text
vm1.contoso.com -> 131.107.50.20
```

Debe crearse manualmente.

---

# Reglas rápidas para el examen AZ-104

| Pregunta | Respuesta |
|---|---|
| Public DNS soporta auto-registration | ❌ No |
| Private DNS soporta auto-registration | ✅ Sí |
| Auto-registration requiere VNet Link | ✅ Sí |
| Azure registra automáticamente Public IPs | ❌ No |
| Azure registra automáticamente Private IPs | ✅ Sí |
| RBAC activa auto-registration | ❌ No |

---

# Frase clave para memorizar

```text
Azure auto-registers VMs ONLY in Private DNS Zones and ONLY with private IP addresses.
```
