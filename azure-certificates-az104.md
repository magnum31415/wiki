[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure App Service Certificates - PKCS#12 vs PEM (AZ-104)

---

# Concepto clave

Azure App Service utiliza certificados SSL/TLS para:

- HTTPS
- Custom domains
- Secure communications

Cuando los certificados se almacenan en:

```text
Azure Key Vault
```

App Service requiere formatos compatibles directamente.

---

# PKCS#12 (PFX)

## Qué es

Formato estándar para certificados SSL/TLS.

Extensiones típicas:

```text
.pfx
.p12
```

---

# Qué contiene

PKCS#12 normalmente incluye:

✅ certificado público  
✅ private key  
✅ certificados intermedios  

todo dentro de un único archivo.

---

# Compatibilidad con Azure App Service

Azure App Service soporta directamente:

```text
PFX / PKCS#12
```

para:

- TLS/SSL bindings
- HTTPS custom domains

---

# Importante examen

Azure App Service requiere:

```text
private key certificates
```

↓

Por eso PFX es el formato recomendado.

---

# PEM

## Qué es

Formato texto Base64.

Extensiones típicas:

```text
.pem
.crt
.key
```

---

# Características

PEM frecuentemente:

❌ separa certificate y private key  
❌ requiere múltiples archivos  
❌ necesita conversión para App Service  

---

# Compatibilidad App Service

PEM:

❌ no se usa directamente para TLS/SSL bindings en App Service  
✅ puede convertirse a PFX  

---

# Conversión típica

```text
PEM → PFX
```

usando:

```text
openssl pkcs12
```

---

# Regla rápida examen

| Formato | Compatible directamente con App Service |
|---|---|
| PFX / PKCS#12 | ✅ |
| PEM | ❌ |
| CER público sin private key | ❌ |

---

# Diferencia importante examen

| Formato | Incluye Private Key |
|---|---|
| PFX / PKCS#12 | ✅ |
| PEM | A veces separada |
| CER | ❌ |

---

# Frases clave AZ-104

```text
Azure App Service requires private key certificates in PFX format.
```

```text
PKCS#12 is the recommended certificate format for Azure App Service.
```

```text
PEM certificates usually require conversion before use in App Service.
```

```text
PFX files contain both the certificate and the private key.
```
