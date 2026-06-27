[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

-# Índice

- [Private DNS Zones](#private-dns-zones)
- [Virtual Network Links](#virtual-network-links)
- [Auto-registration](#auto-registration)
- [Name Resolution en Azure](#name-resolution-en-azure)

---
# Auto-registration

**Auto-registration** es una característica de **Azure Private DNS Zones** que crea, actualiza y elimina automáticamente los registros DNS de las máquinas virtuales de una **Registration Virtual Network**.

## Funcionamiento

Al crear una VM:

```text
VM
 │
 ▼
Private IP
 │
 ▼
Azure crea automáticamente
 │
 ▼
Registro DNS tipo A
```

Ejemplo:

```text
VM1
IP: 10.0.0.5

↓

vm1.contoso.local → 10.0.0.5
```

## Actualización automática

- Si cambia la **Private IP**, Azure actualiza automáticamente el registro DNS.

## Eliminación automática

- Si se elimina la VM, Azure elimina automáticamente el registro DNS.

## Requisitos

Para que **Auto-registration** funcione deben cumplirse **todos** los siguientes requisitos:

- ✅ Debe existir una **Private DNS Zone**.
- ✅ Debe existir un **Virtual Network Link** entre la Private DNS Zone y la VNet.
- ✅ En el **Virtual Network Link** debe estar habilitada la opción **Enable auto registration = Yes**.
- ✅ La VNet pasa a ser una **Registration Virtual Network**.
- ✅ La máquina virtual debe pertenecer a esa **Registration Virtual Network**.
- ✅ La VM debe tener una **Private IP**.

Visualmente:

```text
        Private DNS Zone
         contoso.local
                │
                │
      Virtual Network Link
                │
 Enable auto registration = Yes
                │
                ▼
             VNet1
                │
                ▼
               VM1
                │
                ▼
 Registro DNS creado automáticamente
```

## ¿Qué registra automáticamente?

- ✅ Máquinas virtuales (VMs).

## ¿Qué NO registra?

- ❌ Private Endpoints
- ❌ Load Balancers
- ❌ Azure Firewall
- ❌ Application Gateway
- ❌ Azure Bastion

## Registration vs Resolution

| Tipo de Virtual Network Link | Resuelve DNS | Auto-registration |
|------------------------------|:------------:|:-----------------:|
| **Registration Virtual Network** (`Enable auto registration = Yes`) | ✅ | ✅ |
| **Resolution Virtual Network** (`Enable auto registration = No`) | ✅ | ❌ |

## Reglas para el AZ-104

- Una **Private DNS Zone** puede enlazarse a varias VNets.
- Todas las **Virtual Networks enlazadas** pueden resolver los registros de la zona (**Forward DNS Resolution**).
- Solo las VMs de una **Registration Virtual Network** se registran automáticamente.
- No es necesario **VNet Peering** para que varias VNets enlazadas a la misma **Private DNS Zone** puedan resolver nombres.
- El **VNet Peering** proporciona conectividad IP, pero **no crea registros DNS automáticamente**.
- **Enable auto registration** es una propiedad del **Virtual Network Link**, no de la Private DNS Zone ni de la Virtual Network.

---

# Private DNS Zones

## ¿Qué es una Private DNS Zone?

Una **Private DNS Zone** es una zona DNS privada administrada por Azure que permite resolver nombres DNS **únicamente desde redes privadas**.

No es accesible desde Internet.

Se utiliza para resolver nombres de:

- Máquinas virtuales.
- Private Endpoints.
- Aplicaciones internas.
- Servicios internos.

---

## Arquitectura

```text
                Internet
                     │
             No accesible
                     │
        -------------------------
                     │
              Azure Subscription
                     │
             Private DNS Zone
              contoso.local
                     │
              Virtual Network
                     │
        ┌────────────┴────────────┐
        │                         │
      VM1                      VM2
```

---

## Características

- Servicio administrado.
- Alta disponibilidad.
- Sin necesidad de servidores DNS.
- Resolución privada.
- Integración con Azure.
- Compatible con múltiples VNets.

---

## Casos de uso

- Resolución de nombres internos.
- Private Endpoints.
- Aplicaciones internas.
- Ambientes híbridos.
- Azure Kubernetes Service.
- Azure SQL mediante Private Endpoint.

---

## Diferencias con Public DNS Zone

| Public DNS Zone | Private DNS Zone |
|-----------------|------------------|
| Accesible desde Internet | Solo desde redes privadas |
| Publica registros públicos | Publica registros privados |
| Requiere delegación del dominio | No requiere delegación pública |
| Utilizada para sitios web | Utilizada para recursos internos |

---

# Virtual Network Links

## ¿Qué son?

Una Private DNS Zone no puede resolver nombres para ninguna VNet hasta que exista un:

```
Virtual Network Link
```

---

## Arquitectura

```text
            Private DNS Zone

              contoso.local
                    │
                    │
          Virtual Network Link
                    │
                    ▼
                  VNet1
```

---

## Una zona puede enlazarse a varias VNets

```text
               contoso.local

            ┌──────┼──────┐
            │      │      │
         VNet1  VNet2  VNet3
```

---

## Una VNet puede enlazarse a varias zonas

```text
        VNet1

     ┌────┼─────┐
     │    │     │
 ZoneA ZoneB ZoneC
```

---

## Tipos de Virtual Network Link

Existen dos modos.

---

### Registration Virtual Network

Permite:

- Resolver nombres.
- Registrar automáticamente máquinas virtuales.

---

### Resolution Virtual Network

Permite únicamente:

- Resolver nombres.

No registra automáticamente las máquinas virtuales.

---

## Comparativa

| Tipo | Resolución | Auto-registration |
|-------|------------|-------------------|
| Registration | Sí | Sí |
| Resolution | Sí | No |

---

## Ejemplo

```text
             Private DNS Zone

               contoso.local

                    │

      ┌─────────────┴─────────────┐

 Registration Link          Resolution Link

        │                          │

      VNet1                     VNet2

      VM1                       VM2

Registro automático         Sin registro automático
```

---

# Auto-registration

## ¿Qué es?

Permite registrar automáticamente los registros DNS tipo A de las máquinas virtuales.

---

## Funcionamiento

Cuando se crea una máquina virtual:

```text
VM

↓

Private IP

↓

Azure crea automáticamente

↓

A Record
```

---

## Ejemplo

VM

```
VM1
```

Private IP

```
10.0.0.5
```

Registro creado

```
vm1.contoso.local

10.0.0.5
```

---

## Eliminación automática

Cuando la VM se elimina:

```
VM eliminada

↓

Registro DNS eliminado
```

---

## Actualización automática

Si cambia la IP privada:

```
VM

↓

Nueva Private IP

↓

Registro actualizado
```

---

## Requisitos

- Private DNS Zone.
- Virtual Network Link.
- Registration habilitado.
- Máquina virtual perteneciente a esa VNet.

---

## Solo funciona para máquinas virtuales

No registra automáticamente:

- Load Balancer.
- Private Endpoint.
- Azure Firewall.
- Application Gateway.
- Recursos externos.

---

## Solo una Registration VNet por enlace

Cada enlace configura su propio modo.

---

## Ejemplo típico del examen

```text
Private DNS Zone

        │

Registration

        │

      VNet2

        │

      VM6
```

VM6

```
Registro automático
```

---

VM5

```text
VNet1
```

No se registra automáticamente.

---

# Name Resolution en Azure

## Métodos disponibles

Azure permite resolver nombres mediante:

- Azure-provided DNS.
- Custom DNS.
- Private DNS Zones.
- Azure DNS Private Resolver.

---

## Azure-provided DNS

```text
VM

↓

168.63.129.16

↓

Azure DNS
```

---

## Custom DNS

```text
VM

↓

Servidor DNS corporativo
```

---

## Private DNS Zone

```text
VM

↓

Private DNS Zone

↓

Registro DNS
```

---

## Azure DNS Private Resolver

```text
Azure

↓

Private Resolver

↓

DNS On-Premises
```

---

# Resolución dentro de una VNet

```text
VM1

↓

DNS

↓

VM2
```

Siempre que:

- Exista resolución DNS.
- Existan registros.

---

# Resolución entre VNets

Si dos VNets están enlazadas a la misma zona:

```text
            contoso.local

             │

      ┌──────┴───────┐

    VNet1         VNet2
```

Ambas pueden resolver los registros.

---

# Resolución mediante Peering

El peering no crea registros DNS.

El peering únicamente proporciona conectividad IP.

Para resolver nombres se necesita:

- Private DNS Zone.
- Azure-provided DNS.
- Custom DNS.

---

# Escenario

```text
VNet1 ---- Peering ---- VNet2
```

Sin Private DNS Zone:

```
ping vm2

↓

No resuelve
```

---

Con Private DNS Zone:

```
ping vm2.contoso.local

↓

Resuelve correctamente
```

---

# Resolución híbrida

```text
Azure

↓

Private Resolver

↓

VPN / ExpressRoute

↓

DNS On-Premises
```

---

# Escenarios típicos del AZ-104

## Una zona enlazada a varias VNets

Sí.

---

## Una VNet enlazada a varias zonas

Sí.

---

## Auto-registration

Solo registra máquinas virtuales de la VNet configurada como:

```
Registration Virtual Network
```

---

## Resolution Link

Permite resolver.

No registra.

---

## Peering

No crea registros DNS.

---

## Azure-provided DNS

Puede resolver registros de una Private DNS Zone si la VNet está enlazada.

---

# Resumen para memorizar

## Private DNS Zone

- Resolución privada.
- No accesible desde Internet.
- Compatible con múltiples VNets.

---

## Virtual Network Link

Obligatorio para utilizar una Private DNS Zone.

---

## Registration Link

- Resolución DNS.
- Registro automático.

---

## Resolution Link

- Resolución DNS.
- Sin registro automático.

---

## Auto-registration

Registra automáticamente:

- Nombre de la VM.
- Dirección IP privada.

---

## Elimina automáticamente

Cuando la VM desaparece.

---

## Actualiza automáticamente

Cuando cambia la IP privada.

---

## Peering

Proporciona conectividad.

No crea registros DNS.

---

## Reglas de examen

- Una Private DNS Zone puede enlazarse a múltiples VNets.
- Una VNet puede enlazarse a múltiples Private DNS Zones.
- Auto-registration solo funciona para la VNet configurada como **Registration Virtual Network**.
- Una **Resolution Virtual Network** únicamente resuelve nombres.
- El peering nunca registra automáticamente nombres DNS.
- Las máquinas virtuales solo se registran automáticamente en la zona privada cuando pertenecen a una VNet con **Auto-registration habilitado**.
