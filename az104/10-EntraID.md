[AZ104-INDEX](./readme.md)

# 10 - Microsoft Entra ID (AZ-104)

> Este documento resume la teoría de **Microsoft Entra ID** más preguntada en el examen **AZ-104**.

---

# Índice

- Usuarios
- Grupos
- Tipos de grupos
- Grupos anidados
- Group-Based Licensing
- Dynamic Groups
- Guest Users
- Self-Service Password Reset (SSPR)
- Administrative Roles
- Administrative Units
- Preguntas trampa

---

# 1. ¿Qué es Microsoft Entra ID?

**Microsoft Entra ID** (antes Azure Active Directory) es el servicio de identidad de Azure.

Permite administrar:

- Usuarios.
- Grupos.
- Aplicaciones.
- Autenticación.
- Autorización.

Se integra con todos los servicios de Azure.

---

# 2. Usuarios

Microsoft Entra ID admite distintos tipos de usuarios:

- Member
- Guest

Los usuarios **Member** pertenecen a la organización.

Los usuarios **Guest** pertenecen a organizaciones externas.

---

# 3. Security Groups

Los **Security Groups** se utilizan para:

- Asignar permisos.
- Azure RBAC.
- Licencias.
- SSPR.

Permiten:

- Miembros directos.
- Grupos anidados.

Son el tipo de grupo más utilizado.

---

# 4. Microsoft 365 Groups

Los **Microsoft 365 Groups** están orientados a la colaboración.

Incluyen automáticamente recursos como:

- Outlook.
- Teams.
- SharePoint.
- Planner.

También pueden utilizarse para:

- Licencias.
- SSPR.

**No admiten grupos anidados.**

---

# 5. Comparativa de grupos

| Tipo | Azure RBAC | Licencias | Grupos anidados |
|------|------------|-----------|-----------------|
| Security Group | ✅ | ✅ | ✅ |
| Microsoft 365 Group | Limitado | ✅ | ❌ |

---

# 6. Role-Assignable Groups

Los **Role-Assignable Groups** son Security Groups especiales que pueden utilizarse para asignar roles administrativos de Microsoft Entra ID.

Características:

- Requieren Microsoft Entra ID P1/P2.
- No admiten grupos anidados.
- No pueden convertirse posteriormente desde un grupo normal.

---

# 7. Dynamic Groups

Los **Dynamic Groups** agregan usuarios automáticamente mediante reglas.

Ejemplo:

```
Department = IT
```

Todos los usuarios del departamento IT pasarán automáticamente a formar parte del grupo.

---

# 8. Group-Based Licensing

Permite asignar licencias a un grupo en lugar de hacerlo usuario por usuario.

Ventajas:

- Administración sencilla.
- Asignación automática.
- Eliminación automática.

Es uno de los temas más preguntados del AZ-104.

---

# 9. Miembros directos

Las licencias de **Group-Based Licensing** solo se aplican a:

**Miembros directos.**

Los grupos anidados **no heredan** licencias.

Pregunta clásica del examen.

---

# 10. Reasignación automática

Si un usuario pertenece a un grupo con una licencia asignada:

↓

Se elimina manualmente la licencia del usuario.

↓

Microsoft Entra la volverá a asignar automáticamente.

Mientras siga perteneciendo al grupo, conservará la licencia.

---

# 11. Guest Users

Los **Guest Users** permiten colaborar con usuarios externos.

Se identifican normalmente mediante:

```
user#EXT#
```

Pueden acceder únicamente a los recursos para los que se les conceda permiso.

---

# 12. Self-Service Password Reset (SSPR)

**SSPR** permite que los usuarios restablezcan su contraseña sin intervención del administrador.

Puede habilitarse para:

- Todos los usuarios.
- Security Groups.
- Microsoft 365 Groups.

No puede habilitarse para:

- Mail-enabled Security Groups.

---

# 13. Roles que administran SSPR

Los principales roles administrativos son:

- Global Administrator
- Authentication Policy Administrator

Estos roles pueden configurar las opciones generales de SSPR.

---

# 14. Preguntas de seguridad

Las preguntas de seguridad utilizadas por SSPR pueden administrarse mediante:

- Global Administrator.
- Authentication Administrator.

Es una diferencia habitual en los simulacros.

---

# 15. Administrative Units

Las **Administrative Units** permiten delegar la administración de un subconjunto de usuarios y grupos.

Ejemplo:

```
Barcelona

Madrid

Lisboa
```

Cada administrador gestiona únicamente los objetos de su unidad.

---

# 16. Roles administrativos

Los roles más habituales son:

| Rol | Función |
|------|---------|
| Global Administrator | Administración completa |
| User Administrator | Usuarios y grupos |
| Authentication Administrator | Métodos de autenticación |
| Authentication Policy Administrator | Configuración de SSPR |
| Password Administrator | Restablecimiento de contraseñas |

---

# 17. Licencias

Las licencias pueden asignarse:

- Directamente al usuario.
- Mediante **Group-Based Licensing**.

Microsoft recomienda utilizar la asignación mediante grupos.

---

# 18. Buenas prácticas

Microsoft recomienda:

- Utilizar **Security Groups** para Azure RBAC.
- Asignar licencias mediante **Group-Based Licensing**.
- Utilizar **Dynamic Groups** cuando sea posible.
- Habilitar **SSPR**.
- Aplicar el principio de mínimo privilegio mediante roles administrativos.

---

# Preguntas trampa del AZ-104

✅ **Security Groups** admiten grupos anidados.

✅ **Microsoft 365 Groups** **no** admiten grupos anidados.

✅ **Group-Based Licensing** solo funciona con **miembros directos**.

✅ Si un usuario sigue perteneciendo al grupo, Microsoft Entra volverá a asignarle automáticamente la licencia.

✅ **SSPR** puede habilitarse para **Security Groups** y **Microsoft 365 Groups**, pero no para **Mail-enabled Security Groups**.

✅ Los **Guest Users** pertenecen a organizaciones externas.

✅ Los **Dynamic Groups** utilizan reglas para agregar usuarios automáticamente.

---

# Resumen ejecutivo

| Concepto | Clave para el examen |
|----------|----------------------|
| Member | Usuario interno |
| Guest | Usuario externo |
| Security Group | RBAC, licencias y grupos anidados |
| Microsoft 365 Group | Colaboración |
| Role-Assignable Group | Roles administrativos |
| Dynamic Group | Miembros automáticos |
| Group-Based Licensing | Licencias por grupo |
| SSPR | Restablecimiento de contraseña |
| Administrative Unit | Delegación administrativa |
| Global Administrator | Control total |
| Authentication Administrator | Métodos de autenticación |
| Authentication Policy Administrator | Configuración de SSPR |
