[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)


# Azure - ¿Todos los recursos pertenecen a un Resource Group? (AZ-104)

Un **Resource Group** siempre pertenece directamente a **una única Subscription**.

No puede contener:

- Otro Resource Group.
- Otra Subscription.
- Un Management Group.


## Regla general

La mayoría de los recursos de Azure pertenecen a un **Resource Group**.

Ejemplos:

- Virtual Machines
- Storage Accounts
- VNets
- NSGs
- Public IPs
- Key Vaults
- Recovery Services Vaults
- App Services

```text
Subscription
│
└── Resource Group
      │
      ├── VM
      ├── Storage
      ├── VNet
      └── Key Vault
```

---

# Excepciones importantes

Estos recursos **NO pertenecen a un Resource Group**.

| Recurso | Scope |
|----------|-------|
| **Management Group** | Tenant |
| **Subscription** | Tenant |
| **Microsoft Entra ID (Tenant)** | Tenant |
| **Azure Policy Definition (Tenant/Management Group)** | Tenant o Management Group |
| **Azure RBAC Role Definition** | Tenant |
| **Custom Role Definition** | Tenant |
| **Blueprints** (legacy) | Subscription o Management Group |

---

# Cuidado con las asignaciones (Assignments)

Muchas preguntas del AZ-104 juegan con esto.

Una **Policy Definition** puede vivir a nivel:

```text
Tenant
```

Pero una **Policy Assignment** puede hacerse sobre:

- Management Group
- Subscription
- Resource Group
- Recurso

---

# ¿Y los Resource Groups?

Un Resource Group:

- **No pertenece a otro Resource Group.**
- **Sí pertenece a una Subscription.**

```text
Tenant
│
└── Subscription
      │
      └── Resource Group
```

---

# Ejemplos

## Storage Account

```text
Subscription

↓

RG1

↓

Storage Account
```

✅ Necesita un Resource Group.

---

## Virtual Machine

```text
Subscription

↓

RG1

↓

VM
```

✅ Necesita un Resource Group.

---

## Management Group

```text
Tenant

↓

Management Group
```

❌ No pertenece a ningún Resource Group.

---

## Subscription

```text
Tenant

↓

Subscription
```

❌ No pertenece a ningún Resource Group.

---

# Regla mnemotécnica

```text
¿Es infraestructura de Azure?

↓

Management Group
Subscription
Tenant

↓

NO Resource Group
```

```text
¿Es un recurso que despliegas?

↓

VM
Storage
VNet
Key Vault
SQL

↓

Sí necesita Resource Group
```

---

> [!IMPORTANT]
> **Claves para el AZ-104**
>
> - La **gran mayoría** de recursos de Azure pertenecen a un **Resource Group**.
> - Los **Resource Groups** pertenecen a una **Subscription**.
> - **Management Groups**, **Subscriptions** y el **Microsoft Entra Tenant** **no** pertenecen a ningún Resource Group.
> - Si el examen pregunta por una **VM**, **Storage Account**, **VNet**, **NSG**, **Key Vault**, **Recovery Services Vault** o **App Service**, la respuesta casi siempre será que **deben estar dentro de un Resource Group**.
