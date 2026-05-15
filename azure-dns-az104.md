[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice


- [🔎 Concepto clave: Verificación de dominio en Microsoft Entra ID](#-concepto-clave-verificación-de-dominio-en-microsoft-entra-id)
- [Azure DNS Roles importantes para AZ-104](#azure-dns-roles-importantes-para-az-104)

  ---
  

# 🔎 Concepto clave: Verificación de dominio en Microsoft Entra ID

Cuando agregas un dominio personalizado a **Microsoft Entra ID**, Azure necesita confirmar que:

- Realmente eres propietario del dominio
- Tienes control sobre su configuración DNS

Para hacerlo, Azure te pide que agregues un **registro DNS específico** en tu zona pública.

---

# ✅ Respuesta correcta: MX

Microsoft Entra ID permite verificar un dominio usando:

- **TXT**
- **MX**

En este caso, la respuesta correcta es:

**MX**

Azure genera un registro MX especial que debes agregar en tu DNS.  
Si el registro existe y coincide con el valor esperado, el dominio queda verificado.

# 📚 Tipos de registros DNS y para qué se usan

| Tipo de Registro | Nombre completo | ¿Para qué se usa? | Ejemplo típico | ¿Se usa para verificación en Entra ID / M365? |
|------------------|------------------|-------------------|----------------|----------------------------------------------|
| **A** | Address Record | Asocia un dominio a una dirección IPv4 | `example.com → 192.168.1.10` | ❌ No |
| **AAAA** | IPv6 Address Record | Asocia un dominio a una dirección IPv6 | `example.com → 2001:db8::1` | ❌ No |
| **CNAME** | Canonical Name | Alias de un dominio hacia otro dominio | `www.example.com → example.com` | ❌ No (normalmente) |
| **MX** | Mail Exchange | Define el servidor que recibe correos del dominio | `example.com → mail.example.com` | ✅ Sí |
| **TXT** | Text Record | Almacena texto arbitrario (SPF, verificación dominio, etc.) | `v=spf1 include:spf.protection.outlook.com -all` | ✅ Sí |
| **SOA** | Start of Authority | Información administrativa del dominio (DNS primario, tiempos de refresco, etc.) | Define servidor DNS principal | ❌ No |
| **NS** | Name Server | Indica los servidores DNS autoritativos del dominio | `ns1.contoso.com` | ❌ No |
| **PTR** | Pointer Record | Resolución inversa (IP → nombre de dominio) | `192.168.1.10 → example.com` | ❌ No |
| **SRV** | Service Record | Define ubicación de servicios específicos (puerto + host) | `_sip._tcp.example.com` | ❌ No (generalmente) |
| **CAA** | Certification Authority Authorization | Especifica qué autoridades pueden emitir certificados SSL para el dominio | Autoriza Let's Encrypt, DigiCert, etc. | ❌ No |
| **RRSIG** | DNSSEC Signature | Firma criptográfica para validar registros DNS (DNSSEC) | Firma digital del registro | ❌ No |

---

# 🧠 Puntos clave para examen (AZ-104 / AZ-900)

- **Verificación de dominio en Microsoft Entra ID o Microsoft 365 → TXT o MX**
- **Publicar una web → A o CNAME**
- **Configurar correo → MX**
- **SPF / DKIM / verificación servicios cloud → TXT**
- **Información administrativa DNS → SOA**
- **Alias de dominio → CNAME**

---

# Azure DNS Roles importantes para AZ-104

| Rol RBAC | Servicio | Qué puede hacer | Qué NO puede hacer | Uso típico examen |
|---|---|---|---|---|
| DNS Zone Contributor | Public DNS Zones | Administrar zonas DNS públicas y registros DNS | No administrar RBAC | Gestionar dominios públicos |
| Private DNS Zone Contributor | Private DNS Zones | Administrar zonas DNS privadas y Virtual Network Links | No unir VNets | Linking Private DNS ↔ VNet |
| Network Contributor | Recursos de red | Administrar VNets, NICs, NSGs y join VNets | No administrar DNS Zones | Requerido para DNS VNet Links |
| Contributor | General Azure | Administrar casi todos los recursos | No administrar RBAC | Excesivo para least privilege |
| Reader | General Azure | Ver configuración | No modificar | Auditoría / visualización |
| Owner | General Azure | Control total + RBAC | — | Administración completa |

---

# 1. DNS Zone Contributor

## Qué administra

Zonas DNS públicas:

```text
example.com
```

---

## Qué puede hacer

✅ crear registros A  
✅ crear registros CNAME  
✅ modificar registros MX/TXT  
✅ administrar zonas DNS públicas  

---

## Qué NO puede hacer

❌ administrar RBAC  
❌ permisos IAM  

---

# Importante examen

```text
DNS Zone Contributor
```

↓

solo para:

```text
Public DNS Zones
```

---

# 2. Private DNS Zone Contributor

## Qué administra

Private DNS Zones:

```text
privatelink.database.windows.net
```

---

## Qué puede hacer

✅ administrar zonas privadas  
✅ crear Virtual Network Links  
✅ administrar registros privados  

---

## Qué NO puede hacer

❌ unir VNets  
❌ permisos sobre Virtual Networks  

---

# Importante examen

Para:

```text
Private DNS Zone Link
```

normalmente necesitas además:

```text
Network Contributor
```

---

# 3. Network Contributor

## Qué administra

Recursos de red Azure:

- VNets
- NICs
- NSGs
- Load Balancers
- Route Tables

---

# Permiso clave examen

```text
Microsoft.Network/virtualNetworks/join/action
```

---

# Importante examen

Este permiso es necesario para:

```text
linking VNets
```

incluyendo:

- Private DNS Zone Links
- Private Endpoints

---

# 4. Contributor

## Qué puede hacer

✅ administrar casi todos los recursos Azure  

---

# Problema examen

```text
Contributor
```

frecuentemente:

❌ NO es la mejor respuesta  

porque viola:

```text
Least Privilege Principle
```

---

# 5. Reader

## Qué puede hacer

✅ visualizar configuración  
✅ leer recursos  

---

## Qué NO puede hacer

❌ modificar DNS  
❌ crear registros  

---

# 6. Owner

## Qué puede hacer

✅ control total  
✅ administrar RBAC  
✅ asignar roles  

---

# Importante examen

Solo:

```text
Owner
```

o:

```text
User Access Administrator
```

pueden asignar roles RBAC.

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
Private DNS Zone Contributor
```

puede unir VNets.

❌ Incorrecto.

---

## Trampa 2

Pensar que:

```text
Network Contributor
```

puede administrar DNS Zones.

❌ Incorrecto.

---

## Trampa 3

Elegir:

```text
Contributor
```

cuando el examen pide:

```text
least privilege
```

---

# Diferencia importante examen

| Rol | Public DNS | Private DNS | VNet Join |
|---|---|---|---|
| DNS Zone Contributor | ✅ | ❌ | ❌ |
| Private DNS Zone Contributor | ❌ | ✅ | ❌ |
| Network Contributor | ❌ | ❌ | ✅ |
| Contributor | ✅ | ✅ | ✅ |

---

# Reglas rápidas AZ-104

```text
DNS Zone Contributor manages public DNS zones.
```

```text
Private DNS Zone Contributor manages private DNS zones.
```

```text
Network Contributor provides virtualNetworks/join/action.
```

```text
Private DNS Zone linking requires permissions on both the DNS zone and the VNet.
```
