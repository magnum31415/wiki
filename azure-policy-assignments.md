[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


# Azure Policy: Policy Definition, Policy Assignment y Scope

## 1) Policy Definition con `effect` como variable

```json
{
  "name": "Deny-Or-Audit-Storage-Public-Access",
  "properties": {
    "displayName": "Storage accounts should disable public blob access",
    "description": "Allows auditing or denying Storage Accounts with public blob access enabled.",
    "mode": "Indexed",
    "metadata": {
      "category": "Storage"
    },
    "parameters": {
      "effect": {
        "type": "String",
        "metadata": {
          "displayName": "Effect",
          "description": "Audit, deny or disable the policy."
        },
        "allowedValues": [
          "Audit",
          "Deny",
          "Disabled"
        ],
        "defaultValue": "Audit"
      }
    },
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "field": "Microsoft.Storage/storageAccounts/allowBlobPublicAccess",
            "equals": true
          }
        ]
      },
      "then": {
        "effect": "[parameters('effect')]"
      }
    }
  }
}
```

### ¿Qué hace?

```text
Si alguien crea una Storage Account con allowBlobPublicAccess = true:

- Audit    → Se permite crear el recurso y aparece como Non-Compliant.
- Deny     → Se bloquea la creación del recurso.
- Disabled → La policy no actúa.
```

---

## 2) Policy Assignment Definition o  Library Policy Assignment

Técnicamente, la primera no es una Policy Assignment de Azure. Es una definición de Policy Assignment de la librería ALZ.

Caractrística:

- ✓ Reutilizable
- ✓ Independiente del scope
- ✓ Vive en lib/policy_assignments
- ✓ Forma parte de un Archetype
- ✗ No existe todavía en Azure
- ✗ No tiene scope

````
Policy Assignment Definition
         │
         ▼
Archetype
         │
         ▼
Management Group
````

### Producción (Deny)

```json
{
  "name": "Deny-Storage-Public-Access",
  "properties": {
    "displayName": "Deny public blob access on Storage Accounts",
    "description": "Deny Storage Accounts with public blob access enabled.",
    "policyDefinitionId": "/providers/Microsoft.Management/managementGroups/alz/providers/Microsoft.Authorization/policyDefinitions/Deny-Or-Audit-Storage-Public-Access",
    "parameters": {
      "effect": {
        "value": "Deny"
      }
    }
  }
}
```

### Transición (Audit)

```json
{
  "name": "Audit-Storage-Public-Access",
  "properties": {
    "displayName": "Audit public blob access on Storage Accounts",
    "description": "Audit Storage Accounts with public blob access enabled.",
    "policyDefinitionId": "/providers/Microsoft.Management/managementGroups/alz/providers/Microsoft.Authorization/policyDefinitions/Deny-Or-Audit-Storage-Public-Access",
    "parameters": {
      "effect": {
        "value": "Audit"
      }
    }
  }
}
```

### ¿Qué hace?

```text
Las dos assignments apuntan a la MISMA Policy Definition.

La única diferencia es:

Producción:
effect = Deny

Transición:
effect = Audit
```

---

## 3) Asignación al Management Group - Azure Policy Assignment Resource "Microsoft.Authorization/policyAssignments"

### Corp Enforced

Características:

- ✓ Existe físicamente en Azure
- ✓ Tiene scope
- ✓ Aparece en Azure Portal
- ✓ Aparece en Compliance
- ✓ Se evalúa sobre recursos



```json
{
  "type": "Microsoft.Authorization/policyAssignments",
  "apiVersion": "2024-04-01",
  "name": "Deny-Storage-Public-Access",
  "properties": {
    "displayName": "Deny public blob access on Storage Accounts",
    "policyDefinitionId": "/providers/Microsoft.Management/managementGroups/alz/providers/Microsoft.Authorization/policyDefinitions/Deny-Or-Audit-Storage-Public-Access",
    "scope": "/providers/Microsoft.Management/managementGroups/corp-enforced",
    "parameters": {
      "effect": {
        "value": "Deny"
      }
    }
  }
}
```

### Corp Transition

```json
{
  "type": "Microsoft.Authorization/policyAssignments",
  "apiVersion": "2024-04-01",
  "name": "Audit-Storage-Public-Access",
  "properties": {
    "displayName": "Audit public blob access on Storage Accounts",
    "policyDefinitionId": "/providers/Microsoft.Management/managementGroups/alz/providers/Microsoft.Authorization/policyDefinitions/Deny-Or-Audit-Storage-Public-Access",
    "scope": "/providers/Microsoft.Management/managementGroups/corp-transition",
    "parameters": {
      "effect": {
        "value": "Audit"
      }
    }
  }
}
```

---

## Relación entre los componentes

```text
POLICY DEFINITION
│
└── Deny-Or-Audit-Storage-Public-Access
      │
      ▼

POLICY ASSIGNMENT DEFINITIONS
│
├── Deny-Storage-Public-Access
│      effect = Deny
│
└── Audit-Storage-Public-Access
       effect = Audit
       │
       ▼

ARCHETYPES
│
├── corp
│      │
│      └── Deny-Storage-Public-Access
│
└── corp_transition_custom
       │
       └── Audit-Storage-Public-Access
       │
       ▼

MANAGEMENT GROUPS
│
├── Corp Enforced
│      │
│      └── archetype = corp
│
└── Corp Transition
       │
       └── archetype = corp_transition_custom
```
