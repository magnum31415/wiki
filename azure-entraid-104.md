[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)
# EntraID 

## Index

- [Microsoft Active Directory (On-Premises) vs Microsoft Entra ID](#microsoft-active-directory-on-premises-vs-microsoft-entra-id)
  - [Overview](#overview)
  - [Architecture](#architecture)
  - [Identity Management](#identity-management)
  - [Device Management](#device-management)
  - [Authentication and Security](#authentication-and-security)
  - [Integration](#integration)
  - [Typical Use Cases](#typical-use-cases)
  - [Hybrid Identity (Common Scenario)](#hybrid-identity-common-scenario)
  - [Summary](#summary)

- [Active Directory vs Microsoft Entra ID](#active-directory-vs-microsoft-entra-id)
  - [¿Son lo mismo?](#son-lo-mismo)
  - [Diferencia fundamental](#diferencia-fundamental)
  - [No son intercambiables](#no-son-intercambiables)
  - [Ejemplo práctico](#ejemplo-práctico)
  - [Modelo más común: Identidad híbrida](#modelo-más-común-identidad-híbrida)

- [Microsoft Entra ID Plans and Pricing](#microsoft-entra-id-plans-and-pricing)
  - [Feature Comparison](#feature-comparison)
  - [Typical Use Cases](#typical-use-cases)

# Microsoft Active Directory (On-Premises) vs Microsoft Entra ID

## Overview

| Feature | Microsoft Active Directory (On-Premises) | Microsoft Entra ID |
|---|---|---|
| Type | On-premises directory service | Cloud identity and access management service |
| Former Name | Active Directory Domain Services (AD DS) | Azure Active Directory (Azure AD) |
| Deployment | Installed on Windows Server in local infrastructure | Hosted and managed by Microsoft in Azure |
| Primary Purpose | Manage identities and devices inside a corporate network | Manage identities for cloud applications and services |

---

## Architecture

| Aspect | Active Directory (AD DS) | Microsoft Entra ID |
|---|---|---|
| Infrastructure | Requires domain controllers | No servers required |
| Network dependency | Designed for internal networks (LAN) | Designed for internet and cloud access |
| Protocols | LDAP, Kerberos, NTLM | OAuth2, OpenID Connect, SAML |
| Authentication model | Domain-based authentication | Token-based authentication |

---

## Identity Management

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Users and Groups | Yes | Yes |
| Organizational Units (OU) | Yes | No |
| Group Policy (GPO) | Yes | No |
| Role-Based Access Control | Limited | Yes (RBAC built-in) |
| Conditional Access | No | Yes |

---

## Device Management

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Domain Join | Yes | No |
| Entra ID Join | No | Yes |
| Hybrid Join | Yes (with Entra integration) | Yes |
| Device Management | Group Policy | Intune / MDM |

---

## Authentication and Security

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| Kerberos Authentication | Yes | No |
| Multi-Factor Authentication | Limited | Yes |
| Conditional Access | No | Yes |
| Identity Protection | No | Yes |
| Single Sign-On | Internal SSO | Cloud SSO |

---

## Integration

| Feature | Active Directory | Microsoft Entra ID |
|---|---|---|
| On-premises applications | Yes | Limited |
| Cloud applications | Limited | Yes |
| Microsoft 365 integration | Requires synchronization | Native |
| Hybrid identity | With Entra Connect | Native support |

---

## Typical Use Cases

| Active Directory | Microsoft Entra ID |
|---|---|
| Managing internal corporate networks | Identity management for cloud services |
| Windows domain authentication | Microsoft 365 authentication |
| Managing servers and workstations | SaaS application access |
| Group Policy configuration | Conditional access and identity security |

---

## Hybrid Identity (Common Scenario)

Many organizations use **both systems together**.

Architecture example:

````
On-premises Active Directory
│
│ (Azure AD Connect / Entra Connect)
│
Microsoft Entra ID
│
├── Microsoft 365
├── Azure resources
└── SaaS applications
````


In this model:

- **Active Directory** manages internal infrastructure.
- **Microsoft Entra ID** manages cloud authentication and access.

---

## Summary

| Aspect | Active Directory | Microsoft Entra ID |
|---|---|---|
| Environment | On-premises | Cloud |
| Authentication | Kerberos / LDAP | OAuth / SAML / OpenID |
| Infrastructure | Requires domain controllers | Fully managed by Microsoft |
| Device management | Group Policy | Intune / MDM |
| Cloud integration | Limited | Native |

**Active Directory** is designed for traditional on-premises environments, while **Microsoft Entra ID** is built for cloud identity and modern authentication scenarios.


# Active Directory vs Microsoft Entra ID

## ¿Son lo mismo?

No. **No son lo mismo y no son intercambiables**, aunque ambos gestionen identidades.

Tienen **arquitecturas, protocolos y objetivos diferentes**.

---

## Diferencia fundamental

## Diferencia fundamental


| Aspecto | Active Directory (AD DS) | Microsoft Entra ID |
|---|---|---|
| Tipo | Directorio **on-premises** | Directorio **cloud** |
| Ubicación | Servidores Windows dentro de la red corporativa | Servicio gestionado por Microsoft en Azure |
| Arquitectura | **Jerárquica basada en dominios** (forest, domains, OUs, domain controllers) | **Arquitectura cloud multi-tenant** basada en **tenants e identidades** |
| Protocolos | **LDAP, Kerberos, NTLM, DNS** | **OAuth2, OpenID Connect, SAML, SCIM** |
| Autenticación | Autenticación **basada en dominio** (Kerberos tickets) | Autenticación **basada en tokens** |
| Objetivo | Gestionar **identidades, dispositivos y recursos dentro de la red corporativa** | Gestionar **identidades y acceso a aplicaciones cloud y SaaS** |
| Diseño | Pensado para **redes internas (LAN)** | Pensado para **internet y aplicaciones cloud** |
| Infraestructura | Requiere **Domain Controllers** | No requiere servidores |
| Qué tienen en común | Ambos son **servicios de identidad y directorio** que gestionan **usuarios, grupos y autenticación** | Ambos permiten **gestionar identidades, controlar accesos y aplicar políticas de seguridad** |

---

## No son intercambiables

### Funcionalidades que solo tiene Active Directory

| Solo Active Directory |
|---|
| Domain Join clásico |
| Group Policy (GPO) |
| LDAP |
| Kerberos |
| Gestión de servidores Windows |

---

### Funcionalidades que solo tiene Microsoft Entra ID

| Solo Microsoft Entra ID |
|---|
| Conditional Access |
| MFA integrado |
| Identity Protection |
| Gestión de acceso a aplicaciones SaaS |
| Integración nativa con Microsoft 365 |

---

## Ejemplo práctico

### Login en un servidor Windows interno

Utiliza:

- **Active Directory**
- Kerberos
- Domain Controller

---

### Login en Microsoft 365

Utiliza:

- **Microsoft Entra ID**
- OAuth / tokens
- Autenticación cloud

---

## Modelo más común: Identidad híbrida

La mayoría de empresas utilizan **ambos sistemas conectados**.

# Microsoft Entra ID Plans and Pricing

| Plan | Price (approx.) | Target Use | Key Features |
|---|---|---|---|
| **Free** | Free | Basic identity management for Azure and Microsoft services | User and group management, Single Sign-On (SSO) for Azure and some SaaS apps, Basic security reports, Self-service password change |
| **Microsoft Entra ID P1** | ~ $6 user/month | Enterprise identity management | Conditional Access, Dynamic groups, Self-service password reset (SSPR) for cloud and on-prem users, Hybrid identity with Entra Connect, Advanced group management |
| **Microsoft Entra ID P2** | ~ $9 user/month | Advanced security and identity protection | All P1 features plus Identity Protection, Risk-based Conditional Access, Privileged Identity Management (PIM), Access reviews |
| **Microsoft Entra Suite** | ~ $12 user/month (varies) | Unified identity, access and security | Includes Entra ID P2 plus Identity Governance, Verified ID, Internet Access and Private Access (Zero Trust access solutions) |

---

## Feature Comparison

| Feature | Free | P1 | P2 |
|---|---|---|---|
| User & Group Management | Yes | Yes | Yes |
| Single Sign-On (SSO) | Yes | Yes | Yes |
| Self-Service Password Reset | Basic | Advanced | Advanced |
| Conditional Access | No | Yes | Yes |
| Dynamic Groups | No | Yes | Yes |
| Hybrid Identity | Limited | Yes | Yes |
| Identity Protection | No | No | Yes |
| Privileged Identity Management (PIM) | No | No | Yes |
| Access Reviews | No | No | Yes |

---

## Typical Use Cases

| Plan | Typical Scenario |
|---|---|
| **Free** | Small environments, basic Azure identity management |
| **P1** | Enterprises using Microsoft 365, hybrid identity, Conditional Access policies |
| **P2** | Organizations requiring advanced security, Zero Trust and privileged access management |
