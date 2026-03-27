# Índice

- [Bicep – Guía rápida de uso en un proyecto](#bicep--guía-rápida-de-uso-en-un-proyecto)
  - [1. ¿Qué es Bicep?](#1-qué-es-bicep)
  - [2. Estructura recomendada de proyecto](#2-estructura-recomendada-de-proyecto)
  - [3. Elementos clave en Bicep](#3-elementos-clave-en-bicep)
  - [4. Uso de módulos (recomendado)](#4-uso-de-módulos-recomendado)
  - [5. Crear Resource Group](#5-crear-resource-group)
  - [6. Desplegar Bicep](#6-desplegar-bicep)
  - [7. Uso de parámetros externos](#7-uso-de-parámetros-externos)
  - [8. Validación previa](#8-validación-previa)
  - [9. Idempotencia](#9-idempotencia)
  - [10. Borrar recursos](#10-borrar-recursos)
  - [11. Buenas prácticas](#11-buenas-prácticas)
  - [12. Flujo de trabajo típico](#12-flujo-de-trabajo-típico)
  - [13. Dónde ejecutar](#13-dónde-ejecutar)

- [az deployment](#az-deployment)
  - [¿Qué es?](#qué-es)
  - [¿Qué hace?](#qué-hace)
  - [¿Para qué se usa?](#para-qué-se-usa)
  - [¿Solo sirve para Bicep?](#solo-sirve-para-bicep)
  - [Tipos de deployment](#tipos-de-deployment)
    - [scope](#scope)
    - [mode](#mode)
  - [Comparativa con Terraform](#comparativa-con-terraform)

  
#  Bicep – Guía rápida de uso en un proyecto

## 1. ¿Qué es Bicep?
Bicep es un lenguaje declarativo para desplegar recursos en Azure (sobre ARM).  
Permite definir infraestructura como código de forma más simple y legible.

⚠️ Limitación importante

- La creación de **Subscription** NO se puede hacer con Bicep.

⚠️ Nota RBAC importante

Los roles necesitan ID, no nombre. Ejemplo real (usar en producción): 
``Key Vault Secrets Officer → b86a8fe4-44ce-4948-aee5-eccb2c155cd7``

## 2. Estructura recomendada de proyecto



````text
iac/
 ├── bicep/
 │    ├── main.bicep
 │    ├── parameters/
 │    │    ├── dev.bicepparam
 │    │    └── prod.bicepparam
 │    └── modules/
 │         ├── keyvault.bicep
 │         ├── storage.bicep
 │         └── network.bicep
 └── scripts/
      └── deploy.sh
````

## 3. Elementos clave en Bicep

**Parámetros**
````text
param location string = 'westeurope'
param kvName string
````

**Recursos**
````text
resource kv 'Microsoft.KeyVault/vaults@2023-07-01' = {
  name: kvName
  location: location
  properties: {
    tenantId: tenant().tenantId
    sku: {
      family: 'A'
      name: 'standard'
    }
  }
}
````

## 4. Uso de módulos (recomendado)

````text
module keyvault 'modules/keyvault.bicep' = {
  name: 'deployKeyVault'
  params: {
    kvName: 'kv-test'
    location: 'westeurope'
  }
}
````

**Outputs**
````text
output vaultName string = kv.name
````

## 5. Crear Resource Group

````text
az group create \
  --name rg-test \
  --location westeurope
````
## 6. Desplegar Bicep
````text
az deployment group create \
  --resource-group rg-test \
  --template-file main.bicep \
  --parameters kvName=kv-test-001
````
## 7. Uso de parámetros externos
````text
az deployment group create \
  --resource-group rg-test \
  --template-file main.bicep \
  --parameters @params/dev.bicepparam
````
## 8. Validación previa
````text
az deployment group what-if \
  --resource-group rg-test \
  --template-file main.bicep
````
## 9. Idempotencia
Ejecutar múltiples veces no duplica recursos
Solo aplica cambios necesarios

## 10. Borrar recursos
````text
az group delete \
  --name rg-test \
  --yes \
  --no-wait
````
## 11. Buenas prácticas

- Separar por módulos (network, keyvault, compute)
- Usar naming consistente
- Evitar valores hardcodeados
- Usar parámetros por entorno
- Controlar con Git
- Usar what-if antes de desplegar

## 12. Flujo de trabajo típico

1. Editar Bicep
2. Validar (what-if)
3. Desplegar
4. Verificar
5. Eliminar recursos (si es entorno de pruebas)

## 13. Dónde ejecutar

- Azure Cloud Shell (recomendado para empezar)
- Local (Azure CLI + Bicep instalado)
- CI/CD (Azure DevOps / GitHub Actions)



# az deployment

## ¿Qué es?

`az deployment` es el comando del Azure CLI que permite ejecutar **despliegues declarativos en Azure (ARM - Azure Resource Manager)**.

- az deployment = motor de despliegue de Azure
- Ejecuta templates Bicep o ARM
- Funciona por scopes (RG, subscription, etc.)
- Incremental → no borra nada (default)
- Complete → elimina lo que no esté en el template
- No mantiene estado como Terraform

---

## ¿Qué hace?

- Aplica un template (Bicep o ARM JSON)
- Calcula los cambios necesarios (what-if)
- Crea o actualiza recursos en Azure

---

## ¿Para qué se usa?

- Crear infraestructura (IaC)
- Actualizar recursos existentes
- Validar cambios antes de aplicar (`what-if`)
- Estandarizar despliegues

---

## ¿Solo sirve para Bicep?

No.

| Tipo de template | Soportado |
|-----------------|----------|
| Bicep           | ✅ Sí     |
| ARM JSON        | ✅ Sí     |

> Bicep se compila internamente a ARM JSON

---

## Tipos de deployment 

### scope

```bash
az deployment group create     # Resource Group
az deployment sub create       # Subscription
az deployment mg create        # Management Group
az deployment tenant create    # Tenant
````

| Scope            | Valor                             |
| ---------------- | --------------------------------- |
| Resource Group   | (por defecto)                     |
| Subscription     | `targetScope = 'subscription'`    |
| Management Group | `targetScope = 'managementGroup'` |
| Tenant           | `targetScope = 'tenant'`          |


###  mode

El parámetro --mode define cómo se aplican los cambios respecto a lo que ya existe en Azure.

| Mode        | Comportamiento                                                           | Elimina recursos no definidos | Riesgo |
| ----------- | ------------------------------------------------------------------------ | ----------------------------- | ------ |
| Incremental | Crea y actualiza solo los recursos definidos en el template              | ❌ No                          | Bajo   |
| Complete    | Sincroniza el scope: elimina todo lo que no esté definido en el template | ✅ Sí                          | Alto   |

- **Incremental (default)**: seguro → no borra nada
- **Complete: destructivo** → borra lo que no esté en el Bicep dentro del scope

**Explicación de los modos**
1. Incremental (default)
 - Es el modo por defecto
 - Solo aplica cambios sobre los recursos declarados
 - No elimina nada existente
 - Seguro para entornos compartidos

 👉 Quitar un recurso del Bicep NO lo borra

````bash
az deployment group create \
  --resource-group rg-test \
  --template-file main.bicep \
  --parameters params.json
````
2. Complete
 - Hace que el estado del scope coincida exactamente con el template
 - Elimina cualquier recurso que no esté definido
 - Se aplica al scope del deployment (por ejemplo, el Resource Group)

 👉 Si un recurso existe en el RG pero no en el Bicep → se elimina

 ⚠️ Riesgo:
   - Puede borrar recursos creados manualmente
   - Puede afectar otros módulos o equipos

````bash
az deployment group create \
  --resource-group rg-test \
  --template-file main.bicep \
  --parameters params.json \
  --mode Complete
````
### Comparativa con Terraform
| Concepto  | Azure Deployment | Terraform        |
| --------- | ---------------- | ---------------- |
| State     | ❌ No             | ✅ Sí             |
| Motor     | ARM              | Terraform engine |
| Lenguaje  | Bicep/JSON       | HCL              |
| Ejecución | Azure nativo     | Cliente externo  |



