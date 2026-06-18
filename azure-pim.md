[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# ¿Qué es PIM?

**PIM (Privileged Identity Management)** es una funcionalidad de Microsoft Entra ID que permite que un usuario **sea elegible para un rol**, 
pero **no lo tenga activo permanentemente**.

Sin PIM:
````
Ricard
  └── Contributor (activo 24x7)
````

Con PIM:

````
Ricard
  └── Contributor (Eligible)

          ↓ Activar

Ricard
  └── Contributor (activo 1 hora)
````
La idea es reducir los privilegios permanentes.


¿Vuestra gobernanza permite que el equipo DocScan tenga Contributor temporal en producción mediante PIM?"


# Coste PIM

Sí, **PIM tiene coste de licencia**.

Actualmente, para usar PIM sobre recursos de Azure (suscripciones, resource groups, roles RBAC como Contributor, Owner, etc.) los usuarios que vayan a activar esos roles 
necesitan una licencia de nivel Microsoft Entra ID P2 (antes Azure AD Premium P2).

Ejemplo

Si tienes:

Subscription PROD
    └── Contributor (Eligible via PIM)

Grupo:
SG-DocScan-Prod-Contributor

Usuarios:
- Usuario1
- Usuario2
- Usuario3

Los usuarios que activan el rol mediante PIM deberían disponer de licencia Entra ID P2.


## Lo primero que haría sería comprobar si PIM ya está disponible y en uso en vuestro tenant.

### Método 1: Ver si existe PIM

En el portal Azure:
````
Microsoft Entra ID
 └── Privileged Identity Management
````

Si ves el menú y puedes navegar por:

````
Identity Governance
 ├── Microsoft Entra roles
 ├── Azure resources
 ├── Groups
 └── Approvals
````
es muy probable que vuestra organización tenga licencias adecuadas.

## Método 2:buscar directamente en la barra superior del portal: "Privileged Identity Management"

Si esta disponible:
````
Privileged Identity Management

├── Microsoft Entra roles
├── Groups
└── Azure resources
````
La opción Azure resources es precisamente la que se utiliza para gestionar roles Azure RBAC mediante PIM:


Owner
Contributor
User Access Administrator
Reader
...

sobre:
````
Management Groups
Subscriptions
Resource Groups
Recursos individuales
````

## Método 3: Mirar las licencias del tenant

Si tienes permisos:
````
Microsoft Entra ID
 └── Licenses
      └── All products
````

Busca productos como:

- Microsoft Entra ID P2
- Microsoft Entra Suite
- EMS E5
- Microsoft 365 E5

Si aparecen con licencias asignadas, ya tienes derecho a usar PIM.


# Break-Glass Access Using PIM (Example)

## Objective

Allow a limited set of application administrators to obtain temporary Contributor access to a production Azure subscription during emergency situations (break-glass scenarios).

Example application:

```text
Application: InvoiceProcessor
Subscription: sub-finance-invoiceprocessor-prod
```

Normal deployments continue to be performed through Azure DevOps using a Service Connection backed by a Service Principal or Workload Identity Federation.

Human access is only granted temporarily through PIM when required.

---

# Step 1 - Validate that PIM is Available

Verify that Microsoft Entra ID P2 licensing is available in the tenant.

Navigate to:

```text
Microsoft Entra ID
 └── Privileged Identity Management
      └── Azure Resources
```

Verify that:

- PIM is accessible.
- The target subscription is visible.
- The subscription can be managed through PIM.

Example:

```text
Subscription:
sub-finance-invoiceprocessor-prod
```

---

# Step 2 - Create a Security Group

Create a dedicated Security Group for the application administrators.

Example:

```text
Group Name:
sg-invoiceprocessor-prod-pim-contributor
```

Purpose:

```text
Contains the users allowed to activate
temporary Contributor access during emergencies.
```

Example members:

```text
john.smith@company.com
mary.jones@company.com
alex.brown@company.com
```

The application owner is responsible for identifying which users should belong to this group.

---

# Step 3 - Configure PIM Assignment

Create a PIM assignment linking:

```text
Security Group
        ↓
Azure Role
        ↓
Azure Subscription
```

Example:

```text
Security Group:
sg-invoiceprocessor-prod-pim-contributor

Role:
Contributor

Assignment Type:
Eligible

Scope:
sub-finance-invoiceprocessor-prod
```

Diagram:

```text
sg-invoiceprocessor-prod-pim-contributor
                    │
                    ▼
       Eligible Contributor (PIM)
                    │
                    ▼
sub-finance-invoiceprocessor-prod
```

---

# User Experience

The user does not have Contributor permissions permanently.

Normal state:

```text
User
 └── Member of:
     sg-invoiceprocessor-prod-pim-contributor

Contributor Access:
NO
```

During an incident:

```text
User
 └── Activate Contributor through PIM
```

Activation requirements may include:

```text
- MFA
- Business justification
- Limited duration (e.g. 4 hours)
- Optional approval workflow
```

Temporary state:

```text
User
 └── Contributor
     Scope: sub-finance-invoiceprocessor-prod
```

After the activation period expires:

```text
Contributor Access:
Automatically removed
```

---

# Benefits

## Security

No permanent Contributor permissions.

```text
Eligible -> Active -> Automatically Removed
```

## Auditability

Every activation is logged.

Example:

```text
User: john.smith@company.com
Role: Contributor
Scope: sub-finance-invoiceprocessor-prod
Reason: Production Incident INC-12345
Start: 14:00
End: 18:00
```

## Least Privilege

Users only receive elevated permissions when required.

## Operational Flexibility

Application teams can respond to production incidents without requiring permanent administrative access.

---

# Typical Architecture

```text
Azure DevOps Pipeline
        │
        ▼
Service Connection
        │
        ▼
Service Principal / Federated Identity
        │
        ▼
sub-finance-invoiceprocessor-prod


Application Administrators
        │
        ▼
sg-invoiceprocessor-prod-pim-contributor
        │
        ▼
PIM Eligible Contributor
        │
        ▼
sub-finance-invoiceprocessor-prod
```
