#  Bicep – Guía rápida de uso en un proyecto

## 1. ¿Qué es Bicep?
Bicep es un lenguaje declarativo para desplegar recursos en Azure (sobre ARM).  
Permite definir infraestructura como código de forma más simple y legible.

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
