[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Azure Identity y Networking Concepts (AZ-104)

## Índice

- [Azure Identity y Networking Concepts (AZ-104)](#azure-identity-y-networking-concepts-az-104)
- [Tabla comparativa rápida](#tabla-comparativa-rápida)
- [Managed Identity](#managed-identity)
- [Service Principal](#service-principal)
- [Service Connection (SC)](#service-connection-sc)
- [User Account](#user-account)
- [Private Endpoint](#private-endpoint)
- [Diferencia importante](#diferencia-importante)
- [Comparativa completa](#comparativa-completa)
- [Escenarios típicos AZ-104](#escenarios-típicos-az-104)
- [Trampas típicas examen](#trampas-típicas-examen)
- [Reglas rápidas AZ-104](#reglas-rápidas-az-104)
- [Frases clave AZ-104](#frases-clave-az-104)

---

# Tabla comparativa rápida

| Concepto | Tipo | Uso principal | Ejemplo típico |
|---|---|---|---|
| Managed Identity | Identidad Azure | Recursos Azure | Una VM accede a Key Vault sin passwords |
| Service Principal | Identidad aplicación | Automatización | Terraform despliega recursos Azure |
| Service Connection | Configuración conexión | Azure DevOps | Pipeline Azure DevOps conectado a una subscription |
| User Account | Identidad humana | Administración | admin@contoso.com accede al Azure Portal |
| Private Endpoint | Networking | Acceso privado | Una VM accede a Storage Account usando IP privada |

---

# Managed Identity

## Qué es

Una:

```text
Managed Identity (MI)
```

es una identidad administrada automáticamente por Azure para un recurso Azure.

---

## Idea principal

Permite que un recurso Azure se autentique contra otros servicios Azure:

✅ sin passwords  
✅ sin secrets  
✅ sin credenciales manuales  

---

## Ejemplo típico

```text
Azure VM
    ↓
Managed Identity
    ↓
Key Vault
```

---

## Para qué sirve

| Uso | Ejemplo |
|---|---|
| Acceso Key Vault | Leer secretos |
| Acceso Storage | Blob access |
| Acceso ACR | Pull imágenes |
| Acceso SQL | Autenticación |

---

## Dónde se usa

| Servicio Azure | Soportado |
|---|---|
| Virtual Machines | ✅ |
| App Service | ✅ |
| Azure Container Instances | ✅ |
| AKS | ✅ |
| Functions | ✅ |

---

## Tipos

| Tipo | Explicación |
|---|---|
| System-assigned | Ligada al recurso |
| User-assigned | Reutilizable |

---

## Ventajas

| Ventaja | Explicación |
|---|---|
| Sin passwords | Azure rota credenciales |
| Más seguro | Menos secretos |
| Integración RBAC | Sí |
| Automatización | Excelente |

---

## Importante examen

```text
Managed Identity = autenticación automática Azure
```

---

# Service Principal

## Qué es

Un:

```text
Service Principal
```

es una identidad usada por aplicaciones o automatizaciones.

---

## Idea principal

Es como:

```text
un usuario técnico para aplicaciones
```

---

## Uso típico

| Uso | Ejemplo |
|---|---|
| Terraform | Deploy Azure |
| Azure DevOps | Pipelines |
| GitHub Actions | CI/CD |
| Scripts automatizados | Automation |

---

## Cómo autentica

Normalmente usando:

- Client ID
- Secret
- Certificate

---

## Ejemplo típico

```text
Terraform
    ↓
Service Principal
    ↓
Azure Subscription
```

---

## Diferencia con Managed Identity

| Característica | Managed Identity | Service Principal |
|---|---|---|
| Password manual | ❌ | ✅ normalmente |
| Gestionado Azure | ✅ | ❌ |
| Reutilizable fuera Azure | Limitado | ✅ |
| Ideal Azure-only | ✅ | ❌ |

---

## Importante examen

```text
Service Principal = identidad aplicación/automatización
```

---

# Service Connection (SC)

## Qué es

Una:

```text
Service Connection
```

es una configuración usada por herramientas CI/CD para conectarse a Azure.

---

## Dónde se usa

Principalmente en:

| Herramienta | Uso |
|---|---|
| Azure DevOps | Muy común |
| GitHub Actions | Similar conceptualmente |

---

## Qué contiene normalmente

Una Service Connection normalmente usa:

- Service Principal
- Managed Identity
- Workload Identity Federation

---

## Idea importante

La Service Connection:

❌ NO es una identidad  
✅ usa una identidad para conectarse a Azure  

---

## Ejemplo típico

```text
Azure DevOps Pipeline
    ↓
Service Connection
    ↓
Service Principal
    ↓
Azure Subscription
```

---

## Uso típico

| Escenario | Uso |
|---|---|
| Terraform pipeline | Deploy infraestructura |
| Bicep deployment | Deploy Azure |
| ARM templates | Automatización |
| CI/CD | Azure authentication |

---

## Importante examen

Muchos pipelines Azure DevOps usan:

```text
Service Connection
```

para autenticarse contra Azure.

---

# User Account

## Qué es

Un:

```text
User Account
```

es una identidad humana dentro de Microsoft Entra ID.

---

## Ejemplos

```text
john@contoso.com
admin@contoso.com
```

---

## Para qué sirve

| Uso | Ejemplo |
|---|---|
| Login portal Azure | Administración |
| Microsoft 365 | Correo |
| RBAC | Acceso recursos |
| MFA | Seguridad |

---

## Características

| Feature | Soportado |
|---|---|
| Login interactivo | ✅ |
| MFA | ✅ |
| Conditional Access | ✅ |
| Password reset | ✅ |

---

## Importante examen

```text
User Account = identidad humana
```

---

# Private Endpoint

## Qué es

Un:

```text
Private Endpoint
```

es una interfaz de red privada que conecta un recurso Azure mediante IP privada.

---

## Idea principal

Permite acceder a servicios Azure:

✅ usando red privada  
❌ sin Internet pública  

---

## Ejemplo típico

```text
VM
    ↓
Private Endpoint
    ↓
Storage Account
```

---

## Qué hace

| Función | Soportado |
|---|---|
| IP privada | ✅ |
| Private Link | ✅ |
| Network isolation | ✅ |
| Evitar Internet pública | ✅ |

---

## Qué NO hace

❌ NO da permisos RBAC  
❌ NO autentica usuarios  
❌ NO reemplaza Managed Identity  

---

## Servicios típicos

| Servicio | Soportado |
|---|---|
| Storage Account | ✅ |
| Key Vault | ✅ |
| SQL Database | ✅ |
| Cosmos DB | ✅ |
| ACR | ✅ |

---

## Importante examen

```text
Private Endpoint = networking privado
```

---

# Diferencia importante

| Concepto | Función |
|---|---|
| Managed Identity | Autenticación |
| Service Principal | Identidad automatización |
| Service Connection | Configuración conexión CI/CD |
| User Account | Identidad humana |
| Private Endpoint | Conectividad privada |

---

# Comparativa completa

| Característica | Managed Identity | Service Principal | Service Connection | User Account | Private Endpoint |
|---|---|---|---|---|---|
| Es identidad | ✅ | ✅ | ❌ | ✅ | ❌ |
| Es networking | ❌ | ❌ | ❌ | ❌ | ✅ |
| Usa passwords | ❌ | ✅ normalmente | Puede usar | ✅ | ❌ |
| Azure administra credenciales | ✅ | ❌ | Depende | ❌ | N/A |
| RBAC | ✅ | ✅ | Usa RBAC indirectamente | ✅ | ❌ |
| Private IP | ❌ | ❌ | ❌ | ❌ | ✅ |
| Uso típico | Azure services | Automation | Pipelines | Humanos | Network access |

---

# Escenarios típicos AZ-104

| Escenario | Solución típica |
|---|---|
| VM accede Key Vault | Managed Identity |
| Terraform despliega Azure | Service Principal |
| Azure DevOps pipeline | Service Connection |
| Admin accede Portal | User Account |
| Storage privado | Private Endpoint |

---

# Trampas típicas examen

## Trampa 1

Pensar que:

```text
Private Endpoint = autenticación
```

❌ Incorrecto.

Es networking.

---

## Trampa 2

Pensar que:

```text
Managed Identity = networking
```

❌ Incorrecto.

Es identidad/autenticación.

---

## Trampa 3

Confundir:

```text
Service Principal
```

con:

```text
Managed Identity
```

---

## Trampa 4

Pensar que:

```text
Service Connection = identidad
```

❌ Incorrecto.

Es:

```text
una configuración que usa una identidad
```

---

# Reglas rápidas AZ-104

```text
Managed Identity is used for Azure resource authentication.
```

```text
Private Endpoint provides private network connectivity.
```

```text
Service Principals are commonly used for automation.
```

```text
Service Connections are commonly used in Azure DevOps pipelines.
```

```text
User Accounts represent human identities.
```

---

# Frases clave AZ-104

```text
Managed identities eliminate the need to manage credentials.
```

```text
Private Endpoints use private IP addresses inside VNets.
```

```text
Service Principals are used by applications and automation tools.
```

```text
Service Connections are used by CI/CD tools to connect to Azure.
```

```text
RBAC permissions can be assigned to users, groups, managed identities, and service principals.
```
