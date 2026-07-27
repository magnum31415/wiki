[AZ104-INDEX](./readme.md)

# 17 - Azure DNS (AZ-104)

> Este documento resume la teoría de **Azure DNS** más preguntada en el examen **AZ-104**.

---

# Índice

- Azure DNS
- Public DNS Zones
- Private DNS Zones
- DNS Records
- Virtual Network Links
- Registration Virtual Network
- Resolution Virtual Network
- Auto-registration
- Azure-provided DNS
- Custom DNS
- Private Endpoints
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure DNS?

**Azure DNS** es el servicio administrado de resolución de nombres de Azure.

Permite administrar:

- Zonas DNS públicas.
- Zonas DNS privadas.

Es totalmente compatible con el estándar DNS.

---

# 2. Public DNS Zone

Una **Public DNS Zone** publica nombres accesibles desde Internet.

Ejemplo:

```
contoso.com
```

Puede contener registros:

- A
- AAAA
- CNAME
- MX
- TXT
- NS
- PTR
- SRV

---

# 3. Private DNS Zone

Una **Private DNS Zone** solo es accesible desde las **Virtual Networks** vinculadas.

Se utiliza principalmente con:

- Private Endpoints
- Máquinas virtuales
- Servicios internos

No publica registros en Internet.

---

# 4. Virtual Network Link

Una **Private DNS Zone** debe vincularse a una o varias Virtual Networks mediante un:

**Virtual Network Link**

Sin este vínculo, las máquinas virtuales no podrán resolver los nombres de la zona.

---

# 5. Registration Virtual Network

Una **Registration Virtual Network** permite el **registro automático** de máquinas virtuales.

Cuando una VM se crea o cambia su IP privada:

↓

Azure crea o actualiza automáticamente el registro **A** correspondiente.

Solo puede existir **una Registration VNet por Private DNS Zone**.

---

# 6. Resolution Virtual Network

Una **Resolution Virtual Network** únicamente permite resolver nombres de la zona.

No registra automáticamente máquinas virtuales.

Puede haber múltiples Resolution VNets asociadas a una misma zona.

---

# 7. Auto-registration

La **Auto-registration** registra automáticamente:

- Nombre de la VM.
- Dirección IP privada.

Solo funciona para máquinas virtuales ubicadas en la **Registration Virtual Network**.

No registra:

- Private Endpoints.
- Azure App Service.
- Azure SQL.

---

# 8. Registros creados

La Auto-registration únicamente crea:

**Registros A**

Utilizando la **Private IP** de la máquina virtual.

No crea:

- CNAME
- TXT
- MX
- PTR

---

# 9. Registration también resuelve

La **Registration Virtual Network** también actúa automáticamente como:

**Resolution Virtual Network**

Por tanto:

- Registra nombres.
- Resuelve nombres.

---

# 10. Azure-provided DNS

Todas las Virtual Networks utilizan por defecto el servicio:

**Azure-provided DNS**

Este servicio resuelve:

- Recursos internos.
- Private DNS Zones vinculadas.

No requiere configuración adicional.

---

# 11. Custom DNS

Es posible configurar servidores DNS propios para una Virtual Network.

Ejemplos:

- Windows DNS Server.
- BIND.
- Infoblox.

Cuando se configura un Custom DNS:

↓

Las máquinas virtuales dejan de utilizar Azure-provided DNS.

---

# 12. DNS y Private Endpoint

Cuando se crea un **Private Endpoint**, Microsoft recomienda crear también una:

**Private DNS Zone**

De esta forma:

```
storageaccount.blob.core.windows.net

↓

Private IP
```

Las aplicaciones acceden automáticamente al endpoint privado.

---

# 13. DNS Suffix

El **DNS Suffix** configurado en una máquina virtual **no influye** en la Auto-registration.

Lo único que determina el registro automático es que la VM pertenezca a una:

**Registration Virtual Network**

---

# 14. Private Endpoint y DNS

Los **Private Endpoints** no se registran automáticamente mediante Auto-registration.

Azure crea los registros DNS correspondientes cuando el Private Endpoint se integra con una **Private DNS Zone**.

---

# 15. Casos de uso

Las **Private DNS Zones** se utilizan principalmente para:

- Private Endpoints.
- Aplicaciones internas.
- Azure SQL privado.
- Storage Accounts privados.
- Azure Key Vault privado.

---

# 16. Buenas prácticas

Microsoft recomienda:

- Utilizar **Private DNS Zones** junto con **Private Endpoints**.
- Configurar solo una **Registration Virtual Network** por zona.
- Utilizar **Resolution Virtual Networks** para el resto de VNets.
- Mantener **Azure-provided DNS** salvo necesidad de un DNS personalizado.

---

# Preguntas trampa del AZ-104

✅ Una **Private DNS Zone** solo es accesible desde las **Virtual Networks** vinculadas.

✅ Solo puede existir **una Registration Virtual Network** por Private DNS Zone.

✅ Puede haber **varias Resolution Virtual Networks**.

✅ La **Registration Virtual Network** también actúa como **Resolution Virtual Network**.

✅ La **Auto-registration** únicamente crea registros **A**.

✅ La **Auto-registration** solo registra máquinas virtuales de la **Registration Virtual Network**.

✅ El **DNS Suffix** de la VM **no afecta** a la Auto-registration.

✅ Los **Private Endpoints** normalmente requieren una **Private DNS Zone** para resolver su nombre hacia la **Private IP**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure DNS | Servicio DNS administrado |
| Public DNS Zone | Resolución desde Internet |
| Private DNS Zone | Resolución privada |
| Virtual Network Link | Vincula la VNet a la zona |
| Registration VNet | Registro automático |
| Resolution VNet | Solo resolución |
| Auto-registration | Solo registros A |
| Azure-provided DNS | DNS por defecto |
| Custom DNS | Sustituye al DNS de Azure |
| Private Endpoint | Recomendado con Private DNS Zone |
| DNS Suffix | No afecta al registro automático |
```
