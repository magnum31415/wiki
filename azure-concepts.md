# Azure Concepts

## Azure Hierarchy (top->bottom)
Azure is layered, not account-centric like AWS.
````scss
TENANT (Microsoft Entra ID)
 └── Management Groups (optional)
      └── Subscriptions
           └── Resource Groups
                └── Resources
````

### Tenant = Identity layer

The **tenant** is **only about identity**.
- Lives in Microsoft Entra ID
- Contains:
  - Users
  - Groups
  - Service principals
  - Identity security (MFA, Conditional Access)
  
👉 **The tenant does NOT contain Azure resources.**

### Subscriptions = Resource & billing boundary

A **subscription** is:
- A billing boundary
- A quota / limit boundary
- An RBAC permission boundary

👉 **A subscription is a container**, but a container **for resources**, not identity.

### Relationship between tenant and subscriptions

Fundamental rule:
- 🔒 **Each subscription belongs** to exactly **ONE tenant**
- 🔓 **A tenant can have MANY subscriptions**

````graphql
Tenant A
 ├─ Subscription 1
 ├─ Subscription 2
 └─ Subscription 3
````

There is no such thing as a subscription without a tenant.

### Where does YOUR user fit?

Your user: ````es-adm-mcdonals@companyrz.onmicrosoft.com````

- **Users exist in the tenant, not in subscriptions.**
- **They only gain access to subscriptions through RBAC role assignments**.


