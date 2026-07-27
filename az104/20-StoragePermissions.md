[AZ104-INDEX](./readme.md)

# 20 - Storage Permissions (AZ-104)

> Este documento resume los **roles RBAC específicos de Azure Storage** más preguntados en el examen **AZ-104**.

---

# Índice

- Azure RBAC y Storage
- Management Roles vs Data Roles
- Storage Blob Data Roles
- Storage File Data SMB Roles
- Combinación con Reader
- Principio de mínimo privilegio
- Buenas prácticas
- Preguntas trampa

---

# 1. Azure RBAC en Storage

Azure Storage utiliza **Azure RBAC** para controlar el acceso a los datos.

Los permisos pueden asignarse sobre distintos ámbitos:

- Storage Account
- Blob Container
- File Share

Microsoft recomienda utilizar **Microsoft Entra ID + Azure RBAC** en lugar de Access Keys.

---

# 2. Management Roles vs Data Roles

Es importante distinguir entre ambos tipos de roles.

| Management Roles | Data Roles |
|------------------|------------|
| Administran el recurso | Acceden a los datos |
| Reader | Storage Blob Data Reader |
| Contributor | Storage Blob Data Contributor |
| Owner | Storage Blob Data Owner |

Los **Management Roles** no conceden acceso al contenido de los blobs o archivos.

---

# 3. Reader

El rol **Reader** permite:

- Ver el Storage Account.
- Consultar la configuración.
- Visualizar métricas.

No permite acceder al contenido de los datos.

---

# 4. Contributor

El rol **Contributor** permite administrar el Storage Account.

Puede:

- Modificar la configuración.
- Crear Containers.
- Eliminar el recurso.

No permite leer ni escribir blobs mediante Microsoft Entra ID.

---

# 5. Owner

El rol **Owner** incluye todos los permisos de Contributor y además permite:

- Asignar roles RBAC.
- Administrar permisos.

Es el rol con mayores privilegios.

---

# 6. Storage Blob Data Reader

Permite:

- Leer blobs.
- Descargar blobs.
- Listar Containers.

No permite:

- Crear.
- Modificar.
- Eliminar.

Es el rol recomendado para usuarios de solo lectura.

---

# 7. Storage Blob Data Contributor

Permite:

- Leer.
- Crear.
- Modificar.
- Eliminar blobs.

No permite administrar permisos RBAC.

Es el rol más utilizado para aplicaciones.

---

# 8. Storage Blob Data Owner

Incluye todos los permisos del **Storage Blob Data Contributor**.

Además puede:

- Administrar permisos.
- Modificar ACLs.

Está orientado a administradores.

---

# 9. Storage File Data SMB Share Reader

Permite acceder a un **Azure File Share** mediante SMB en modo:

- Solo lectura.

No permite modificar archivos.

---

# 10. Storage File Data SMB Share Contributor

Permite:

- Leer.
- Crear.
- Modificar.
- Eliminar archivos.

No permite cambiar permisos NTFS.

Es el rol habitual para usuarios.

---

# 11. Storage File Data SMB Share Elevated Contributor

Incluye todos los permisos del Contributor.

Además permite:

- Modificar permisos NTFS.
- Administrar ACLs.

Está orientado a administradores del File Share.

---

# 12. Reader + Data Role

Para trabajar desde el **Azure Portal** normalmente se necesita combinar:

```
Reader

+

Storage Blob Data Contributor
```

o

```
Reader

+

Storage Blob Data Reader
```

El rol **Reader** permite visualizar el recurso y el **Data Role** permite acceder a los datos.

---

# 13. Scope recomendado

Los Data Roles pueden asignarse sobre:

- Storage Account
- Blob Container
- File Share

Microsoft recomienda utilizar el **Scope más reducido posible**, siguiendo el principio de mínimo privilegio.

---

# 14. Buenas prácticas

Microsoft recomienda:

- Utilizar **Microsoft Entra ID**.
- Evitar Access Keys cuando sea posible.
- Asignar permisos mediante grupos.
- Utilizar el Scope más pequeño posible.
- Conceder únicamente los permisos necesarios.

---

# Preguntas trampa del AZ-104

✅ **Reader** no permite acceder al contenido de los blobs.

✅ **Contributor** administra el recurso, pero no concede acceso a los datos.

✅ Los **Data Roles** son necesarios para acceder a blobs y Azure Files mediante Microsoft Entra ID.

✅ Para trabajar con Blob Storage desde el Portal suele ser necesario **Reader + Storage Blob Data Contributor**.

✅ **Storage Blob Data Owner** puede administrar permisos sobre los blobs.

✅ **Storage File Data SMB Share Elevated Contributor** puede modificar permisos NTFS.

---

# Resumen ejecutivo

| Rol | Permisos |
|------|----------|
| Reader | Solo configuración |
| Contributor | Administra el recurso |
| Owner | Administra recurso + RBAC |
| Storage Blob Data Reader | Leer blobs |
| Storage Blob Data Contributor | Leer y escribir blobs |
| Storage Blob Data Owner | Administrar datos y permisos |
| Storage File Data SMB Share Reader | Leer archivos |
| Storage File Data SMB Share Contributor | Leer y escribir archivos |
| Storage File Data SMB Share Elevated Contributor | Administrar permisos NTFS |

---

# Regla para el examen

Cuando una pregunta mencione:

- **Microsoft Entra ID**
- **Mínimo privilegio**
- **Acceso a blobs o archivos**

La respuesta correcta casi siempre será un **Storage Data Role** (Blob o File), y **no** un rol genérico como **Contributor** u **Owner**.
