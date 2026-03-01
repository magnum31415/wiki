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

Si quieres, puedo prepararte una segunda tabla solo con los que suelen caer en exámenes de Azure (más simplificada para memorizar rápido).
