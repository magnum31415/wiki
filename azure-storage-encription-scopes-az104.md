[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Storage Encryption - Storage Service Encryption vs Encryption Scopes (AZ-104)

---

# Azure Storage Encryption

Azure Storage cifra automáticamente los datos almacenados usando:

```text
Storage Service Encryption (SSE)
```

---

# 1. Storage Service Encryption (SSE)

## Qué es

Mecanismo de cifrado automático de Azure Storage.

Azure cifra datos:

✅ en reposo  
✅ automáticamente  
✅ transparente para aplicaciones  

---

# Qué recursos protege

SSE protege:

| Servicio | Compatible |
|---|---|
| Blob Storage | ✅ |
| Azure Files | ✅ |
| Queues | ✅ |
| Tables | ✅ |

---

# Qué claves puede usar

SSE puede usar:

| Tipo de clave | Descripción |
|---|---|
| Microsoft-managed keys (MMK) | Claves gestionadas por Microsoft |
| Customer-managed keys (CMK) | Claves almacenadas normalmente en Key Vault |

---

# Características importantes

- habilitado por defecto
- cifrado transparente
- aplica a todo el Storage Account

---

# Ejemplo

```text
Storage Account = storage1
```

↓

Todo el contenido queda cifrado:

- blobs
- files
- queues
- tables

---

# 2. Encryption Scopes

## Qué es

Permiten definir cifrado específico para:

```text
Blob Storage
```

a nivel más granular.

---

# Qué recursos soporta

| Recurso | Compatible |
|---|---|
| Blob Containers | ✅ |
| Blobs | ✅ |
| Azure Files | ❌ |
| Queues | ❌ |
| Tables | ❌ |

---

# Para qué sirve

Permite:

- separar datos sensibles
- usar distintas claves
- aplicar compliance granular
- cifrado diferente dentro del mismo Storage Account

---

# Características importantes

- solo Blob Storage
- asociado a un único Storage Account
- granularidad container/blob

---

# Ejemplo

```text
storage2
```

↓

```text
Encryption Scope = Scope1
```

↓

Aplicado únicamente a:

- cont1
- blob1

pero NO necesariamente al resto del Storage Account.

---

# Diferencia clave examen

| Feature | Scope |
|---|---|
| Storage Service Encryption | Storage Account completo |
| Encryption Scope | Blob container/blob |

---

# Comparativa examen

| Característica | Storage Service Encryption (SSE) | Encryption Scope |
|---|---|---|
| Nivel | Storage Account | Blob/Container |
| Blob Storage | ✅ | ✅ |
| Azure Files | ✅ | ❌ |
| Queues | ✅ | ❌ |
| Tables | ✅ | ❌ |
| Microsoft-managed keys | ✅ | ✅ |
| Customer-managed keys | ✅ | ✅ |
| Granularidad fina | ❌ | ✅ |
| Configuración por blob/container | ❌ | ✅ |

---

# Ejemplo comparativo

## SSE

```text
storage1
```

↓

Todo cifrado automáticamente:

- blobs
- files
- queues
- tables

---

## Encryption Scope

```text
storage2
```

↓

Solo ciertos blobs/contenedores usan:

```text
Scope1
```

---

# Regla rápida examen

```text
Storage Service Encryption protects the entire storage account.
```

```text
Encryption Scopes provide granular encryption for Blob Storage.
```

```text
Encryption Scopes support only containers and blobs.
```

---

# Frases clave AZ-104

```text
Storage Service Encryption is enabled by default in Azure Storage.
```

```text
Encryption scopes provide encryption at the blob or container level.
```

```text
Azure Files, Queues, and Tables do not support encryption scopes.
```
