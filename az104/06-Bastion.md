[AZ104-INDEX](./readme.md)

# 06 - Azure Bastion (AZ-104)

> Este documento resume la teoría de **Azure Bastion** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure Bastion?
- Arquitectura
- AzureBastionSubnet
- Public IP
- SKUs
- Native Client
- VNet Peering
- Limitaciones
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure Bastion?

**Azure Bastion** es un servicio PaaS que permite conectarse mediante **RDP** o **SSH** a máquinas virtuales **sin necesidad de asignarles una Public IP**.

La conexión se realiza desde:

- Azure Portal
- Cliente nativo (RDP/SSH) *(solo Standard)*
- Azure CLI *(solo Standard)*
- Azure PowerShell *(solo Standard)*

---

# 2. Arquitectura

La arquitectura típica es:

```
Usuario

↓

Azure Portal

↓

Azure Bastion

↓

Virtual Network

↓

Virtual Machine
```

Las máquinas virtuales únicamente necesitan una **Private IP**.

---

# 3. AzureBastionSubnet

Toda Virtual Network que aloje un Bastion debe contener una Subnet llamada exactamente:

```
AzureBastionSubnet
```

El nombre **es obligatorio**.

No puede utilizarse otro nombre.

---

# 4. Tamaño de AzureBastionSubnet

Microsoft recomienda que la subnet tenga un tamaño mínimo de:

```
/26
```

Aunque versiones antiguas permitían **/27**, para el examen AZ-104 la respuesta correcta es **/26**.

---

# 5. Public IP

Azure Bastion requiere una **Public IP** con las siguientes características:

- **Standard SKU**
- **Static**
- **IPv4**
- **Regional**

No admite:

- Basic SKU
- Dynamic IP

---

# 6. Bastion Basic

El SKU **Basic** permite:

- RDP desde Azure Portal.
- SSH desde Azure Portal.

No permite:

- Cliente nativo.
- Azure CLI.
- Azure PowerShell.
- File Transfer.
- Session Recording.

---

# 7. Bastion Standard

El SKU **Standard** añade funcionalidades como:

- Native Client Support.
- Azure CLI.
- Azure PowerShell.
- RDP (mstsc).
- SSH desde terminal.
- File Transfer.
- Session Recording.
- IP Connect.
- Shareable Links.

Es el SKU recomendado para producción.

---

# 8. Native Client

Para utilizar:

- **Remote Desktop (mstsc)**
- Cliente SSH
- Azure CLI
- Azure PowerShell

es necesario:

- SKU **Standard**
- Habilitar la característica **Native Client Support**

Pregunta muy frecuente.

---

# 9. Máquinas virtuales compatibles

Azure Bastion permite conectarse a máquinas virtuales que:

- estén en la **misma VNet**
- o en una **VNet directamente peered**

No es necesario asignar Public IP a las máquinas virtuales.

---

# 10. VNet Peering

Azure Bastion puede acceder a máquinas situadas en una **VNet directamente peered**.

No puede atravesar:

- Peerings transitivos.

Ejemplo:

```
Bastion

↓

VNet A

↓

Peering

↓

VNet B

✔ Permitido
```

```
VNet A

↓

VNet B

↓

VNet C

✘ No permitido
```

El **VNet Peering no es transitivo**.

---

# 11. Public IP en las máquinas virtuales

Cuando se utiliza Azure Bastion:

**Las máquinas virtuales no necesitan Public IP.**

Esto reduce la superficie de ataque.

---

# 12. NSG

Las máquinas virtuales protegidas mediante Bastion pueden seguir utilizando **Network Security Groups (NSG)**.

No es necesario abrir el puerto:

- 3389 (RDP)
- 22 (SSH)

hacia Internet.

---

# 13. Azure Bastion vs Jump Box

| Azure Bastion | Jump Box |
|---------------|----------|
| Servicio PaaS | Máquina Virtual |
| Sin mantenimiento | Requiere administración |
| Alta disponibilidad | Depende de la VM |
| Sin Public IP en las VMs | Normalmente requiere Public IP |

Microsoft recomienda utilizar **Azure Bastion**.

---

# 14. Casos de uso

Azure Bastion es la solución recomendada cuando:

- No se desea exponer máquinas virtuales a Internet.
- Se necesita acceso RDP o SSH seguro.
- Se quiere reducir la superficie de ataque.

---

# 15. Buenas prácticas

Microsoft recomienda:

- Utilizar **Standard SKU**.
- Crear una **AzureBastionSubnet /26**.
- Utilizar una **Standard Public IP Static**.
- No asignar Public IP a las máquinas virtuales.
- Utilizar **Native Client** cuando sea necesario acceder mediante mstsc o SSH nativo.

---

# Preguntas trampa del AZ-104

✅ La subnet debe llamarse exactamente **AzureBastionSubnet**.

✅ El tamaño mínimo recomendado es **/26**.

✅ Azure Bastion requiere una **Standard Public IP**, **Static**, **IPv4** y **Regional**.

✅ Las máquinas virtuales **no necesitan Public IP**.

✅ El **Native Client** solo está disponible en **Standard SKU**.

✅ Para utilizar **mstsc**, **Azure CLI** o **Azure PowerShell** debe habilitarse **Native Client Support**.

✅ Azure Bastion funciona con máquinas de la **misma VNet** o de **VNets directamente peered**.

✅ **No soporta peerings transitivos**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure Bastion | Acceso RDP/SSH sin Public IP |
| AzureBastionSubnet | Nombre obligatorio |
| Tamaño mínimo | **/26** |
| Public IP | Standard, Static, IPv4, Regional |
| Basic SKU | Solo Portal |
| Standard SKU | Native Client + CLI + PowerShell |
| Native Client | Requiere Standard + habilitar la característica |
| Public IP en VM | No necesaria |
| VNet Peering | Solo directo |
| Peering transitivo | No soportado |
| Puerto 3389/22 | No es necesario abrirlos a Internet |
