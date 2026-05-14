[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Key Vault y Azure Container Apps (AZ-104)

## Índice

- [Azure Key Vault y Azure Container Apps (AZ-104)](#azure-key-vault-y-azure-container-apps-az-104)
- [Escenario](#escenario)
- [Qué quiere el ejercicio](#qué-quiere-el-ejercicio)
- [Respuesta correcta](#respuesta-correcta)
- [Qué es Azure Key Vault](#qué-es-azure-key-vault)
- [Qué puede almacenar Key Vault](#qué-puede-almacenar-key-vault)
- [Qué es Azure Container Apps](#qué-es-azure-container-apps)
- [Relación entre Container Apps y Key Vault](#relación-entre-container-apps-y-key-vault)
- [Cómo accede Container Apps a Key Vault](#cómo-accede-container-apps-a-key-vault)
- [Importante examen](#importante-examen)
- [Región y Resource Group](#región-y-resource-group)
- [Concepto importante](#concepto-importante)
- [Qué limita realmente el acceso](#qué-limita-realmente-el-acceso)
- [Qué necesita realmente Container App](#qué-necesita-realmente-container-app)
- [Ejemplo conceptual](#ejemplo-conceptual)
- [Trampa típica AZ-104](#trampa-típica-az-104)
- [Otra trampa típica](#otra-trampa-típica)
- [Buenas prácticas vs limitaciones técnicas](#buenas-prácticas-vs-limitaciones-técnicas)
- [Tabla resumen examen](#tabla-resumen-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

## Escenario

Tenemos:

| Recurso | Tipo |
|---|---|
| containerApp01 | Azure Container App |
| Vault1 | Azure Key Vault |
| Vault2 | Azure Key Vault |
| Vault3 | Azure Key Vault |

Los vaults están:

- en distintas regiones
- en distintos Resource Groups

---

## Qué quiere el ejercicio

Microsoft pregunta:

```text
¿Qué Key Vault puede suministrar secretos a containerApp01?
```

---

## Respuesta correcta

✅

```text
Vault1, Vault2 o Vault3
```

---

## Qué es Azure Key Vault

Azure Key Vault es un servicio para almacenar secretos de forma segura.

---

## Qué puede almacenar Key Vault

| Tipo | Ejemplo |
|---|---|
| Secrets | Passwords |
| Keys | Encryption keys |
| Certificates | TLS/SSL |
| Tokens | API tokens |

---

## Qué es Azure Container Apps

Servicio serverless para ejecutar contenedores modernos.

---

## Uso típico

- Microservicios
- APIs
- Containers
- Apps cloud-native

---

## Relación entre Container Apps y Key Vault

Container Apps puede usar:

```text
secrets almacenados en Key Vault
```

---

## Cómo accede Container Apps a Key Vault

Normalmente mediante:

```text
Managed Identity
```

---

## Flujo típico

```text
Container App
    ↓
Managed Identity
    ↓
Key Vault
    ↓
Secret
```

---

## Importante examen

Microsoft NO exige que:

- Key Vault esté en la misma región
- Key Vault esté en el mismo Resource Group

---

## Región y Resource Group

En Azure:

| Elemento | ¿Limita acceso? |
|---|---|
| Región diferente | ❌ |
| Resource Group diferente | ❌ |
| Subscription diferente | Puede depender |
| Tenant diferente | Mucho más complejo |

---

## Concepto importante

Azure Resources pueden comunicarse:

✅ entre regiones  
✅ entre Resource Groups  

si tienen:

- networking correcto
- permisos correctos

---

## Qué limita realmente el acceso

Lo importante NO es:

❌ la región  
❌ el Resource Group  

Lo importante es:

✅ autenticación  
✅ autorización  
✅ networking  

---

## Qué necesita realmente Container App

Para acceder al secreto necesita:

| Requisito | Necesario |
|---|---|
| Acceso red | ✅ |
| Permisos Key Vault | ✅ |
| Secret existente | ✅ |

---

## Ejemplo conceptual

```text
ContainerApp01
    ↓
Managed Identity
    ↓
Vault3 (otra región)
    ↓
Secret
```

✅ Funciona.

---

## Trampa típica AZ-104

Muchos candidatos creen:

```text
Los recursos deben estar en la misma región
```

❌ Incorrecto.

---

## Otra trampa típica

Muchos creen:

```text
Los recursos deben estar en el mismo Resource Group
```

❌ Incorrecto.

Resource Groups son:

```text
contenedores lógicos
```

NO límites de conectividad.

---

## Buenas prácticas vs limitaciones técnicas

## Best Practice

Microsoft recomienda:

✅ misma región  

por:

- menor latencia
- mejor rendimiento
- resiliencia

---

## Pero técnicamente

Azure permite:

✅ acceso cross-region  
✅ acceso cross-resource-group  

---

## Tabla resumen examen

| Concepto | Importante |
|---|---|
| Key Vault cross-region | ✅ Soportado |
| Key Vault cross-resource-group | ✅ Soportado |
| Managed Identity | Muy importante |
| Resource Group = límite acceso | ❌ Incorrecto |
| Región = límite acceso | ❌ Incorrecto |

---

## Reglas rápidas AZ-104

```text
Azure resources can access Key Vaults across regions and resource groups.
```

```text
Resource Groups are logical containers, not security boundaries.
```

```text
Managed identities are commonly used to access Key Vault secrets.
```

---

## Frases clave AZ-104

```text
Azure Key Vault can be accessed from resources in different regions.
```

```text
Container Apps commonly use Managed Identities to access Key Vault.
```

```text
Resource Groups do not restrict Azure resource communication.
```
