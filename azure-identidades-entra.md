# Microsoft Entra ID: App Registration, Service Principal, Enterprise Application, Service Connection y sus Identificadores

# Introducción

Uno de los conceptos que más confusión genera en Microsoft Entra ID (Azure AD) es la diferencia entre:

- App Registration
- Service Principal
- Enterprise Application
- Service Connection (Azure DevOps)
- Application ID (Client ID)
- Object ID

La clave para entenderlo es pensar que:

- Una **App Registration** define una aplicación.
- Un **Service Principal** representa la identidad de esa aplicación dentro de un tenant.
- Una **Enterprise Application** es simplemente la representación del Service Principal en el portal de Microsoft Entra ID.
- Una **Service Connection** es un objeto propio de Azure DevOps que utiliza un Service Principal para autenticarse contra Azure.

---

# 1. Definiciones sencillas

## 1.1 App Registration

Es la definición lógica de una aplicación.

Contiene información como:

- Nombre
- Redirect URI
- API Permissions
- Secrets
- Certificates
- Application ID (Client ID)

No representa una identidad que pueda recibir permisos RBAC sobre Azure Resources.

### Ejemplo

```text
Nombre:
sc-alz-connectivity-apply

Application ID:
1f386f75-bc02-4197-9a0d-5237fec42cc5
```

Puede imaginarse como:

```text
Es el "molde" o definición de la aplicación.
```

---

## 1.2 Service Principal

Es la identidad real de la aplicación dentro de un tenant.

Es el objeto que:

- inicia sesión
- obtiene tokens
- puede autenticarse
- puede recibir permisos RBAC

Tiene su propio:

- Object ID

Cada tenant tiene su propio Service Principal.

Puede imaginarse como:

```text
Es la "persona" que representa a la aplicación.
```

---

## 1.3 Enterprise Application

En Microsoft Entra ID:

```text
Enterprise Application == Service Principal
```

Son exactamente el mismo objeto.

Simplemente Microsoft utiliza nombres distintos dependiendo del contexto.

Cuando entras en:

```text
Microsoft Entra ID

    Enterprise Applications

        sc-alz-connectivity-apply
```

estás viendo realmente el Service Principal.

---

## 1.4 Service Connection (Azure DevOps)

Una Service Connection es un objeto propio de Azure DevOps.

No existe en Microsoft Entra ID.

Su función es almacenar la configuración necesaria para que un Pipeline pueda autenticarse contra Azure.

Dependiendo del tipo de autenticación puede utilizar:

- Service Principal + Secret
- Service Principal + Certificate
- Workload Identity Federation (OIDC)
- Managed Identity

En el caso de Workload Identity Federation:

```text
Azure DevOps Pipeline
        │
        ▼

Service Connection

        │
        ▼

Service Principal

        │
        ▼

Azure Resources
```

Ejemplo:

```text
Nombre:
sc-alz-connectivity-apply

Authentication:
Workload Identity Federation

Utiliza el Service Principal:

Application ID:
1f386f75-bc02-4197-9a0d-5237fec42cc5
```

La Service Connection NO es una identidad.

Simplemente utiliza una identidad para autenticarse.

---

## 1.5 Application ID (Client ID)

Es un identificador global de la aplicación.

Ejemplo:

```text
Application ID

1f386f75-bc02-4197-9a0d-5237fec42cc5
```

Características:

- identifica la aplicación
- no cambia
- es igual en todos los tenants
- se usa para autenticación OAuth
- también se llama Client ID

Se utiliza por ejemplo en:

```text
az login --service-principal

client_id

Azure DevOps Service Connection

OAuth
```

---

## 1.6 Object ID

Es el identificador único de un objeto dentro de un tenant.

Cada objeto tiene su propio Object ID.

Por ejemplo:

```text
Usuario

Object ID:
11111111-...

Grupo

Object ID:
22222222-...

Service Principal

Object ID:
8cb8568f-79ed-45ca-9d4d-594d3990ab66
```

Este identificador es el que utiliza Azure RBAC internamente.

---

# 2. Relación entre todos ellos

```text
                    App Registration

              Nombre:
              sc-alz-connectivity-apply

              Application ID
              1f386f75-bc02-4197-9a0d-5237fec42cc5

                            │
                            │ crea
                            ▼

                 Service Principal
                 (Enterprise Application)

              Nombre:
              sc-alz-connectivity-apply

              Object ID
              8cb8568f-79ed-45ca-9d4d-594d3990ab66

                            ▲
                            │
                            │ utiliza
                            │

                 Azure DevOps Service Connection

              Nombre:
              sc-alz-connectivity-apply

              Authentication:
              Workload Identity Federation
```

---

# 3. Ejemplo completo

Supongamos que Azure DevOps crea automáticamente una Service Connection mediante Workload Identity Federation.

Se crean o utilizan los siguientes objetos:

## App Registration

```text
Nombre

sc-alz-connectivity-apply

Application ID

1f386f75-bc02-4197-9a0d-5237fec42cc5
```

---

## Enterprise Application (Service Principal)

```text
Nombre

sc-alz-connectivity-apply

Application ID

1f386f75-bc02-4197-9a0d-5237fec42cc5

Object ID

8cb8568f-79ed-45ca-9d4d-594d3990ab66
```

---

## Azure DevOps Service Connection

```text
Nombre

sc-alz-connectivity-apply

Authentication

Workload Identity Federation

Utiliza:

Application ID:
1f386f75-bc02-4197-9a0d-5237fec42cc5

Para autenticarse como el Service Principal cuyo:

Object ID:
8cb8568f-79ed-45ca-9d4d-594d3990ab66
```

---

# 4. Ejemplo completo con RBAC

```text
                  Azure DevOps Pipeline
                              │
                              ▼

                   Service Connection

                  sc-alz-connectivity-apply

                              │
                              ▼

                     App Registration

                  Application ID

          1f386f75-bc02-4197-9a0d-5237fec42cc5

                              │
                              ▼

            Service Principal (Enterprise Application)

                  Object ID

          8cb8568f-79ed-45ca-9d4d-594d3990ab66

                              │
                              │ principal_id
                              ▼

                   Azure Role Assignment

                Storage Blob Data Contributor

                              │
                              ▼

                       Azure Storage Account
```

---

# 5. ¿Qué identificador debo usar con Terraform para asignar Roles?

La respuesta es:

```text
Siempre el Object ID del Service Principal.
```

Nunca el Application ID.

## Correcto

```hcl
resource "azurerm_role_assignment" "example" {

  scope                = azurerm_storage_account.example.id

  role_definition_name = "Storage Blob Data Contributor"

  principal_id         = "8cb8568f-79ed-45ca-9d4d-594d3990ab66"
}
```

---

## Incorrecto

```hcl
resource "azurerm_role_assignment" "example" {

  scope                = azurerm_storage_account.example.id

  role_definition_name = "Storage Blob Data Contributor"

  principal_id         = "1f386f75-bc02-4197-9a0d-5237fec42cc5"
}
```

---

# 6. ¿Por qué Terraform necesita el Object ID?

Porque Azure RBAC asigna permisos a identidades existentes dentro del tenant.

Y esas identidades son:

- Usuarios
- Grupos
- Managed Identities
- Service Principals

Todos ellos están identificados mediante su:

```text
Object ID
```

Internamente el Role Assignment es conceptualmente equivalente a:

```text
Role Assignment

Role:
    Storage Blob Data Contributor

Principal:
    Object ID = 8cb8568f-79ed-45ca-9d4d-594d3990ab66

Scope:
    /subscriptions/.../storageAccounts/stnetlogsprodgwc001
```

No almacena el Application ID.

---

# 7. Resumen rápido

| Concepto | ¿Qué es? | Existe en Entra ID | Identificador principal |
|-----------|----------|-------------------|--------------------------|
| App Registration | Definición lógica de una aplicación | Sí | Application ID (Client ID) |
| Service Principal | Identidad de la aplicación dentro de un tenant | Sí | Object ID |
| Enterprise Application | Otro nombre para el Service Principal | Sí | Object ID |
| Service Connection | Configuración de autenticación de Azure DevOps | No | No tiene Object ID de Entra propio |
| Application ID | Identificador global de la aplicación | Sí | Igual en todos los tenants |
| Object ID | Identificador único de un objeto dentro de un tenant | Sí | Distinto para cada objeto |

---

# 8. Regla mnemotécnica

## App Registration

```text
Define QUÉ es la aplicación.
```

Tiene:

```text
Application ID
```

---

## Service Principal / Enterprise Application

```text
Representa QUIÉN ejecuta la aplicación.
```

Tiene:

```text
Object ID
```

y es quien:

- inicia sesión
- obtiene tokens
- recibe permisos RBAC
- aparece en IAM
- ejecuta Terraform
- ejecuta Azure DevOps
- ejecuta GitHub Actions

---

## Service Connection

```text
Es simplemente la configuración que utiliza Azure DevOps
para autenticarse utilizando un Service Principal.
```

No es una identidad de Microsoft Entra ID.

---

# Regla práctica definitiva

```text
OAuth
OIDC
OpenID Connect
Client ID
az login
Service Connection
Authentication
        │
        ▼

Application ID (Client ID)

----------------------------------------------------

IAM
RBAC
Terraform principal_id
Role Assignment
Storage Blob Data Contributor
Owner
Contributor
Azure Roles
        │
        ▼

Object ID del Service Principal
```
