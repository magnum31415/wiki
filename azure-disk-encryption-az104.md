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
- [Azure Disk Encryption (ADE) - Compatibilidad y requisitos AZ-104](#azure-disk-encryption-ade---compatibilidad-y-requisitos-az-104)
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


---
# Azure Disk Encryption (ADE) - Compatibilidad y requisitos AZ-104

## Índice

- [Azure Disk Encryption (ADE) - Compatibilidad y requisitos AZ-104](#azure-disk-encryption-ade---compatibilidad-y-requisitos-az-104)
- [Qué es Azure Disk Encryption](#qué-es-azure-disk-encryption)
- [Qué protege ADE](#qué-protege-ade)
- [Sistemas operativos soportados](#sistemas-operativos-soportados)
- [Tipos de discos soportados](#tipos-de-discos-soportados)
- [Qué NO soporta ADE](#qué-no-soporta-ade)
- [Por qué VM2 y VM3 son correctas](#por-qué-vm2-y-vm3-son-correctas)
- [Por qué VM1 es incorrecta](#por-qué-vm1-es-incorrecta)
- [Por qué VM4 es incorrecta](#por-qué-vm4-es-incorrecta)
- [Por qué VM5 es incorrecta](#por-qué-vm5-es-incorrecta)
- [Conceptos importantes examen](#conceptos-importantes-examen)
- [Diferencia importante](#diferencia-importante)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Trampas típicas AZ-104](#trampas-típicas-az-104)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Qué es Azure Disk Encryption

Azure Disk Encryption (ADE) permite cifrar:

- OS Disks
- Data Disks

de máquinas virtuales Azure.

---

# Qué protege ADE

ADE cifra:

| Elemento | Soportado |
|---|---|
| OS Disk | ✅ |
| Data Disks | ✅ |

---

# Sistemas operativos soportados

| Sistema operativo | Soportado |
|---|---|
| Windows Server | ✅ |
| Linux compatibles (RHEL, Ubuntu, etc.) | ✅ |

---

# Tipos de discos soportados

| Tipo disco | Soportado |
|---|---|
| Standard HDD | ✅ |
| Standard SSD | ✅ |
| Premium SSD | ✅ |

---

# Qué NO soporta ADE

| Feature | Soportado |
|---|---|
| Ephemeral OS Disk | ❌ |
| Write Accelerator | ❌ |
| Dynamic Volumes | ❌ |

---

# Por qué VM2 y VM3 son correctas

## VM2

Usa:

- Windows Server 2022
- Basic volume

↓

✅ Compatible con ADE

---

## VM3

Usa:

- Red Hat Enterprise Linux
- Standard SSD

↓

✅ Compatible con ADE

---

# Por qué VM1 es incorrecta

VM1 utiliza:

```text
Ephemeral OS Disk
```

---

## Importante

ADE NO soporta:

```text
Ephemeral OS Disks
```

porque estos discos:

- son temporales
- están ligados al host físico
- no tienen persistencia tradicional

---

# Por qué VM4 es incorrecta

VM4 utiliza:

```text
Write Accelerator
```

---

## Qué es Write Accelerator

Feature de alto rendimiento para discos Premium.

---

## Importante

ADE NO soporta:

```text
Write Accelerator
```

---

# Por qué VM5 es incorrecta

VM5 utiliza:

```text
Dynamic Volume
```

---

# Diferencia importante

| Tipo volumen | Soportado ADE |
|---|---|
| Basic Volume | ✅ |
| Dynamic Volume | ❌ |

---

# Qué es un Basic Volume

Volumen estándar tradicional Windows.

---

# Qué es un Dynamic Volume

Volumen avanzado Windows con features como:

- spanning
- striping
- mirroring software

---

# Importante examen

ADE soporta:

✅ Basic volumes  
❌ Dynamic volumes  

---

# Conceptos importantes examen

Microsoft suele evaluar:

| Concepto | Importancia |
|---|---|
| ADE compatibility | Muy alta |
| Ephemeral OS Disk | Alta |
| Write Accelerator | Alta |
| Dynamic Volume | Alta |
| Linux compatibility | Alta |

---

# Diferencia importante

| Feature | Compatible ADE |
|---|---|
| Standard SSD | ✅ |
| Premium SSD | ✅ |
| Ephemeral OS Disk | ❌ |
| Write Accelerator | ❌ |
| Dynamic Volumes | ❌ |

---

# Tabla resumen examen

| VM característica | Compatible con ADE |
|---|---|
| Windows + Basic Volume | ✅ |
| Linux + Standard SSD | ✅ |
| Ephemeral OS Disk | ❌ |
| Write Accelerator | ❌ |
| Dynamic Volume | ❌ |

---

# Trampas típicas AZ-104

## Trampa 1

Pensar que:

```text
todos los discos Azure soportan ADE
```

❌ Incorrecto.

---

## Trampa 2

Confundir:

```text
Basic Volume
```

con:

```text
Dynamic Volume
```

---

## Trampa 3

Pensar que:

```text
Write Accelerator mejora ADE
```

❌ Incorrecto.

No es compatible.

---

## Trampa 4

Pensar que Linux no soporta ADE.

❌ Incorrecto.

Muchos Linux sí están soportados.

---

# Reglas rápidas AZ-104

```text
Azure Disk Encryption supports Windows and supported Linux distributions.
```

```text
Ephemeral OS disks are not supported by ADE.
```

```text
Dynamic volumes are not supported by ADE.
```

```text
Write Accelerator is incompatible with ADE.
```

---

# Frases clave AZ-104

```text
Azure Disk Encryption supports basic volumes.
```

```text
Ephemeral OS disks cannot be encrypted using ADE.
```

```text
Write Accelerator disks are not compatible with Azure Disk Encryption.
```
