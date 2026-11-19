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

# 15. Azure RBAC Conditions (AZ-104)

## ¿Qué son?

Las **Azure RBAC Conditions** permiten **restringir aún más** los permisos concedidos por un rol de Azure RBAC.

Es decir:

> **Rol + Condición = Permisos más específicos**

No modifican el rol, sino que añaden reglas que deben cumplirse para que el acceso sea permitido.

---

# ¿Para qué sirven?

Las Conditions implementan el principio de **Least Privilege (Mínimo Privilegio)**.

En lugar de crear múltiples roles personalizados, es posible utilizar un rol existente y limitar cuándo o sobre qué recursos puede actuar.

---

# Ejemplo básico

Se asigna el rol:

````text
Storage Blob Data Contributor
````

Sin condiciones:

````
Puede:

✔ Leer blobs
✔ Crear blobs
✔ Modificar blobs
✔ Eliminar blobs
````

Con una condición:

````text
Solo puede acceder a blobs cuyo nombre empiece por:

documentos/
````

Resultado:

````
documentos/factura.pdf      ✔ Permitido
documentos/contrato.pdf     ✔ Permitido

imagenes/logo.png           ❌ Denegado
backup.zip                  ❌ Denegado
````

---

# ¿Cómo funciona?

Azure evalúa:

````
¿Tiene el usuario el rol?
↓
Sí
↓
¿Se cumple la condición?
↓
Sí → Acceso permitido
No → Acceso denegado
````

---

# ¿Qué se puede restringir?

Dependiendo del servicio, las Conditions pueden limitar el acceso según:

- Nombre del contenedor.
- Nombre del blob.
- Atributos del recurso.
- Atributos de la solicitud.
- Atributos del principal (usuario o identidad).

---

# Servicios compatibles

Las RBAC Conditions están orientadas principalmente al acceso a datos.

El caso más importante para el AZ-104 es:

- Azure Storage

---

# Roles compatibles (más habituales)

Ejemplos:

- Storage Blob Data Reader
- Storage Blob Data Contributor
- Storage Blob Data Owner

---

# Ejemplo práctico

Sin condición:

````
Usuario

↓

Storage Blob Data Contributor

↓

Container1
````

Puede modificar cualquier blob.

---

Con condición:

````
Usuario

↓

Storage Blob Data Contributor

↓

Condición:

@Resource[BlobPath] empieza por "clientes/"

↓

Container1
````

Puede modificar:

````
clientes/cliente1.pdf
clientes/cliente2.pdf
````

No puede modificar:

````
imagenes/logo.png
backups/full.zip
````

---

# Ventajas

- Aplica el principio de mínimo privilegio.
- Reduce la necesidad de crear roles personalizados.
- Mayor seguridad.
- Permite controlar el acceso a nivel de datos.

---

# Limitaciones

- No todos los roles de Azure admiten Conditions.
- No todos los servicios son compatibles.
- Se utilizan principalmente con los **roles de datos** (Data Plane), especialmente Azure Storage.
- No sustituyen a Azure Policy.

---

# Azure RBAC Conditions vs Azure Policy

| Azure RBAC Conditions | Azure Policy |
|-----------------------|--------------|
| Controlan **quién** puede acceder | Controla **qué recursos** pueden crearse o configurarse |
| Actúan sobre permisos | Actúa sobre cumplimiento (governance) |
| Se aplican a Role Assignments | Se aplican mediante Policy Assignments |
| Principalmente Data Plane | Management Plane |

---

# Claves para el AZ-104

- Las **RBAC Conditions** permiten restringir aún más un rol existente.
- Se utilizan principalmente con **Azure Storage**.
- Son una forma de aplicar el principio de **Least Privilege**.
- No todos los roles admiten Conditions.
- No deben confundirse con **Azure Policy**.


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
