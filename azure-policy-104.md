[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

## Index

- [Minimum Azure Policy JSON](#minimum-azure-policy-json)
- [Required fields](#required-fields)
- [Common optional fields](#common-optional-fields)
- [Typical (but not minimal) policy example](#typical-but-not-minimal-policy-example)
- [Summary](#summary)

---

## Minimum Azure Policy JSON

The **minimum valid JSON document for an Azure Policy** requires only `properties`, `displayName`, and `policyRule` with an `if` condition and a `then` effect.

```json
{
  "properties": {
    "displayName": "Minimal policy",
    "policyRule": {
      "if": {
        "field": "type",
        "equals": "Microsoft.Resources/subscriptions/resourceGroups"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## Required fields

- **properties**
- **displayName**
- **policyRule**
- **if**
- **then.effect**

## Common optional fields

These fields are commonly included but are **not required**:

```json
"description": "Policy description",
"mode": "All",
"parameters": {}
```

## Typical (but not minimal) policy example

```json
{
  "properties": {
    "displayName": "Deny specific resource type",
    "description": "Example policy",
    "mode": "All",
    "policyRule": {
      "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## Summary

The **absolute minimal structure** for an Azure Policy is:

- `properties`
- `displayName`
- `policyRule`
- `if`
- `then.effect`
