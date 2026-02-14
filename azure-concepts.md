[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
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

#### Identity & directory roles (Tenant level)

How to check in the Azure Portal

Go to Microsoft Entra ID

Go to:
````graphql
Users
 └── Select your user
      └── Assigned roles
````

What you’ll see:

Directory roles assigned directly or via groups

If this page is empty → you are not a tenant admin

📌 Important

Having NO directory roles does NOT prevent you from seeing Azure resources.

## Azure RBAC roles (Resource access)

These control what you can see or do with Azure resources
They apply to:
- Management Groups
- Subscriptions
- Resource Groups
- Individual resources

**How to check in the Azure Portal**

**Option A — From Subscriptions**

1. Go to Subscriptions
2. Select a subscription
3. Go to:
````graphql
Access control (IAM)
 └── Role assignments
````
4. Search for your user

Check:
- Role (Reader, Contributor, Owner)
- Scope (Subscription, Resource Group, etc.)

**Option B — From Management Groups (important)**

If you can see many subscriptions, your role is likely inherited.

1. Go to Management Groups
2. Select a group
3. Go to:
````graphql
Access control (IAM)
````
If your user appears here → access is **inherited by all child subscriptions**.
