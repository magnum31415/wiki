[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Storage Encryption Scopes (AZ-104)

---

# Concepto clave

La pregunta evalúa:

```text
qué recursos soportan Azure Encryption Scopes
```

---

# Qué es un Encryption Scope

Un:

```text
Encryption Scope
```

permite definir claves de cifrado específicas para datos almacenados en Azure Blob Storage.

---

# Para qué sirve

Permite:

- usar diferentes claves de cifrado
- separar datos sensibles
- aplicar distintos requerimientos compliance
- controlar cifrado a nivel granular

---

# Importante examen

Encryption Scopes aplican únicamente a:

```text
Azure Blob Storage
```

↓

Específicamente:

✅ containers  
✅ blobs  

---

# Recursos soportados

| Recurso | Compatible con Encryption Scope |
|---|---|
| Blob Containers | ✅ |
| Blobs | ✅ |
| File Shares | ❌ |
| Queues | ❌ |
| Tables | ❌ |

---

# Relación con el caso

El caso dice:

```text
In storage2, create an encryption scope named Scope1
```

↓

Entonces:

```text
Scope1
```

solo puede proteger recursos Blob Storage dentro de:

```text
storage2
```

---

# Por qué la respuesta correcta es

```text
containers and blobs in storage2 only
```

Porque:

✅ Encryption Scopes funcionan en Blob Storage  
✅ Scope1 fue creado en storage2  
✅ containers y blobs sí soportan encryption scopes  

---

# Por qué NO storage1

Porque:

```text
Scope1 existe únicamente en storage2
```

↓

Un Encryption Scope pertenece a:

```text
un único Storage Account
```

---

# Importante examen

Encryption Scopes:

❌ NO son globales  
❌ NO pueden compartirse entre Storage Accounts  

---

# Por qué NO File Shares

Azure Files utiliza otro modelo de cifrado.

↓

Encryption Scopes:

```text
NO soportan Azure Files
```

---

# Por qué NO Queues ni Tables

Queues y Tables:

❌ no soportan Encryption Scopes  

aunque sí utilizan:

```text
Storage Service Encryption (SSE)
```

---

# Trampa típica AZ-104

Pensar que:

```text
Encryption Scope = Storage Account encryption
```

❌ Incorrecto.

---

# Diferencia importante

| Feature | Nivel |
|---|---|
| Storage Service Encryption | Storage Account completo |
| Encryption Scope | Blob container/blob |

---

# Otra trampa típica

Pensar que:

```text
Azure Files soporta Encryption Scopes
```

❌ Incorrecto.

---

# Arquitectura conceptual

```text
Storage Account
    ↓
Encryption Scope
    ↓
Blob Container
    ↓
Blob
```

---

# Regla rápida examen

```text
Encryption Scopes only support Blob Storage resources.
```

```text
Encryption Scopes apply to containers and blobs only.
```

```text
Encryption Scopes belong to a single storage account.
```

---

# Frases clave AZ-104

```text
Encryption scopes provide encryption at the blob or container level.
```

```text
Encryption scopes are supported only for Blob Storage.
```

```text
Azure Files, Queues, and Tables do not support encryption scopes.
```
