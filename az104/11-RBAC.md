[AZ104-INDEX](./readme.md)

# 11 - Azure RBAC (AZ-104)

> Este documento resume la teoría de **Azure Role-Based Access Control (RBAC)** más preguntada en el examen **AZ-104**.

---

# Índice

- ¿Qué es Azure RBAC?
- Scope
- Herencia de permisos
- Built-in Roles
- Role Assignments
- Principio de mínimo privilegio
- RBAC vs Microsoft Entra Roles
- Role Assignment Conditions
- Buenas prácticas
- Preguntas trampa

---

# 1. ¿Qué es Azure RBAC?

**Azure Role-Based Access Control (RBAC)** es el sistema de autorización de Azure que permite controlar quién puede realizar acciones sobre los recursos.

Un permiso RBAC se compone de tres elementos:

- **Quién** (Usuario, Grupo, Service Principal o Managed Identity)
- **Qué rol**
- **Sobre qué ámbito (Scope)**

---

# 2. Scope (Ámbito)

Los roles pueden asignarse en distintos niveles.

```
Management Group
      │
Subscription
      │
Resource Group
      │
Resource
```

Los permisos **se heredan hacia abajo**.

---

# 3. Herencia

Si un usuario recibe un rol en una **Subscription**, automáticamente tendrá esos permisos sobre:

- Todos los Resource Groups.
- Todos los recursos de la Subscription.

No es necesario volver a asignar el rol en cada recurso.

---

# 4. Built-in Roles

Los roles integrados más importantes son:

| Rol | Permisos |
|------|----------|
| Owner | Administración completa + RBAC |
| Contributor | Administra recursos |
| Reader | Solo lectura |
| User Access Administrator | Administra RBAC |

Estos cuatro aparecen constantemente en el examen.

---

# 5. Owner

El rol **Owner** permite:

- Crear recursos.
- Modificar recursos.
- Eliminar recursos.
- Asignar roles RBAC.

Es el único rol general que combina administración de recursos y permisos.

---

# 6. Contributor

El rol **Contributor** puede administrar prácticamente todos los recursos.

**No puede:**

- Asignar roles RBAC.
- Modificar permisos.

Esta es una de las preguntas más frecuentes.

---

# 7. Reader

El rol **Reader** únicamente permite visualizar recursos.

No puede:

- Crear.
- Modificar.
- Eliminar.

Se utiliza habitualmente junto con otros roles específicos de datos.

---

# 8. User Access Administrator

Permite administrar:

- Role Assignments.
- Azure RBAC.

**No puede administrar recursos.**

Es el rol recomendado cuando solo se desea delegar la administración de permisos.

---

# 9. Roles de datos (Data Roles)

Algunos servicios utilizan roles específicos para acceder a los datos.

Ejemplos:

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner
- Storage File Data SMB Share Contributor

Estos roles **no sustituyen** a Reader o Contributor sobre el recurso.

---

# 10. RBAC y Azure Portal

Para administrar datos desde el Portal suele ser necesario combinar:

- **Reader**
- Rol de datos correspondiente

Ejemplo:

```
Reader

+

Storage Blob Data Contributor
```

Sin **Reader**, el usuario puede no visualizar correctamente el recurso en el Portal.

---

# 11. Microsoft Entra Roles vs Azure RBAC

No deben confundirse.

| Microsoft Entra ID | Azure RBAC |
|--------------------|------------|
| Usuarios | Recursos Azure |
| Grupos | Máquinas Virtuales |
| Licencias | Storage |
| MFA | Networking |

Los roles de Microsoft Entra administran la identidad.

Los roles RBAC administran los recursos.

---

# 12. Role Assignment

Una asignación RBAC relaciona:

```
Principal

↓

Rol

↓

Scope
```

Sin una asignación de rol, el usuario no tendrá permisos sobre el recurso.

---

# 13. Role Assignment Conditions

Las **Conditions** permiten restringir cuándo puede utilizarse un rol.

Ejemplo:

Permitir acceso únicamente a determinados **Blob Containers**.

Es una funcionalidad avanzada de Azure RBAC.

---

# 14. Managed Identities

Las **Managed Identities** también pueden recibir roles RBAC.

Esto permite que una aplicación acceda a recursos de Azure sin almacenar credenciales.

---

# 15. Principio de mínimo privilegio

Microsoft recomienda asignar siempre el rol con los permisos mínimos necesarios.

Ejemplo:

- Reader antes que Contributor.
- Contributor antes que Owner.

---

# 16. Elevate Access

Un **Global Administrator** puede utilizar la opción **Elevate Access** para obtener temporalmente el rol:

**User Access Administrator**

sobre el **Tenant Root Management Group**.

Esto permite recuperar el acceso a recursos cuando no existen administradores RBAC.

---

# 17. Buenas prácticas

Microsoft recomienda:

- Asignar permisos a **grupos**, no a usuarios individuales.
- Utilizar el **menor Scope posible**.
- Aplicar el principio de **mínimo privilegio**.
- Evitar asignar **Owner** salvo que sea imprescindible.

---

# Preguntas trampa del AZ-104

✅ **Owner** puede administrar recursos y asignar roles.

✅ **Contributor** **no** puede asignar roles RBAC.

✅ **Reader** solo permite visualizar recursos.

✅ **User Access Administrator** administra permisos, **no recursos**.

✅ Los permisos RBAC **se heredan** desde el Scope superior.

✅ Los roles de **Microsoft Entra ID** no sustituyen a los roles **Azure RBAC**.

✅ Los servicios como **Azure Storage** utilizan **Data Roles** específicos.

✅ Para trabajar con Blob Storage desde el Portal suele necesitarse **Reader + Storage Blob Data Contributor**.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Azure RBAC | Control de acceso a recursos |
| Scope | Management Group → Subscription → Resource Group → Resource |
| Herencia | Sí, hacia abajo |
| Owner | Recursos + RBAC |
| Contributor | Recursos, no RBAC |
| Reader | Solo lectura |
| User Access Administrator | Solo RBAC |
| Data Roles | Acceso a datos (Storage, etc.) |
| Role Assignment | Principal + Rol + Scope |
| Managed Identity | Puede recibir RBAC |
| Elevate Access | Global Admin → User Access Administrator |
| Mínimo privilegio | Recomendación de Microsoft |
