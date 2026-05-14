[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Disk Encryption (ADE) y Key Encryption Key (KEK) - AZ-104

## Índice

- [Azure Disk Encryption (ADE) y Key Encryption Key (KEK) - AZ-104](#azure-disk-encryption-ade-y-key-encryption-key-kek---az-104)
- [Qué es Azure Disk Encryption (ADE)](#qué-es-azure-disk-encryption-ade)
- [Qué cifra realmente ADE](#qué-cifra-realmente-ade)
- [Qué es una Key Encryption Key (KEK)](#qué-es-una-key-encryption-key-kek)
- [Qué tipo de claves soporta KEK](#qué-tipo-de-claves-soporta-kek)
- [RSA vs EC Keys](#rsa-vs-ec-keys)
- [Por qué la respuesta correcta usa Key1](#por-qué-la-respuesta-correcta-usa-key1)
- [Por qué Key2 es incorrecta](#por-qué-key2-es-incorrecta)
- [Cmdlet correcto para ADE](#cmdlet-correcto-para-ade)
- [Flujo conceptual](#flujo-conceptual)
- [Qué almacena Key Vault](#qué-almacena-key-vault)
- [Diferencia importante examen](#diferencia-importante-examen)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué es Azure Disk Encryption (ADE)

Azure Disk Encryption permite cifrar:

- OS Disks
- Data Disks

de máquinas virtuales Azure.

---

# Qué cifra realmente ADE

ADE utiliza:

```text
BitLocker (Windows)
```

o:

```text
DM-Crypt (Linux)
```

---

# Concepto importante

ADE utiliza dos tipos de claves:

| Tipo clave | Función |
|---|---|
| BEK (BitLocker Encryption Key) | Cifra el disco |
| KEK (Key Encryption Key) | Cifra la BEK |

---

# Qué es una Key Encryption Key (KEK)

La KEK es una clave adicional almacenada en:

```text
Azure Key Vault
```

utilizada para proteger la clave principal del cifrado.

---

# Flujo conceptual

```text
Disk
 ↓
BEK cifra disco
 ↓
KEK cifra la BEK
 ↓
Key Vault
```

---

# Qué tipo de claves soporta KEK

Para Azure Disk Encryption:

✅ RSA keys soportadas  
❌ EC (Elliptic Curve) no soportadas  

---

# RSA vs EC Keys

| Tipo clave | Soportado para ADE |
|---|---|
| RSA | ✅ |
| EC (Elliptic Curve) | ❌ |

---

# Por qué la respuesta correcta usa Key1

Key1 era:

```text
RSA 4096
```

y Azure Disk Encryption requiere:

```text
RSA key
```

para la KEK.

---

# Por qué Key2 es incorrecta

Key2 era:

```text
EC key
```

y Azure Disk Encryption NO soporta:

```text
Elliptic Curve keys
```

como KEK.

---

# Cmdlet correcto para ADE

El cmdlet correcto es:

```powershell
Set-AzVMDiskEncryptionExtension
```

---

# Qué hace este cmdlet

Configura Azure Disk Encryption en una VM.

---

# Ejemplo conceptual

```powershell
Set-AzVMDiskEncryptionExtension
```

↓

```text
habilita ADE
configura Key Vault
configura KEK
configura cifrado VM
```

---

# Qué almacena Key Vault

Azure Key Vault puede almacenar:

| Objeto | Uso |
|---|---|
| Secrets | Passwords |
| Certificates | TLS/SSL |
| Keys | Encryption keys |

---

# Diferencia importante examen

| Concepto | Función |
|---|---|
| ADE | Cifrado discos VM |
| Key Vault | Almacenar KEK |
| KEK | Proteger clave cifrado |
| RSA key | Compatible ADE |
| EC key | NO compatible ADE |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
cualquier key de Key Vault sirve
```

❌ Incorrecto.

ADE requiere:

✅ RSA  

---

## Trampa 2

Confundir:

```text
BEK
```

con:

```text
KEK
```

---

## Trampa 3

Pensar que:

```text
EC keys funcionan para ADE
```

❌ Incorrecto.

---

## Trampa 4

Confundir cmdlets.

Microsoft suele inventar opciones falsas como:

```powershell
Set-AzDiskEncryptionKey
```

❌ No existe.

---

# Tabla resumen examen

| Elemento | Correcto para ADE |
|---|---|
| RSA Key | ✅ |
| EC Key | ❌ |
| Set-AzVMDiskEncryptionExtension | ✅ |
| Azure Key Vault | ✅ |

---

# Reglas rápidas AZ-104

```text
Azure Disk Encryption requires RSA keys for KEK.
```

```text
EC keys are not supported for ADE.
```

```text
Set-AzVMDiskEncryptionExtension configures Azure Disk Encryption.
```

---

# Frases clave AZ-104

```text
Azure Disk Encryption uses Key Vault to store encryption keys.
```

```text
A Key Encryption Key (KEK) protects the disk encryption key.
```

```text
RSA keys are supported for Azure Disk Encryption.
```
