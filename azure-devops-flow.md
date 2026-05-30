[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Índice

- [Pipelines](#pipelines)
- [¿Qué es un Pull Request?](#qué-es-un-pull-request)

---
# Pipelines

En vuestro caso (Azure DevOps + Terraform + Azure Landing Zone), normalmente tenéis dos pipelines separadas:

- **CI (Continuous Integration)** → valida los cambios.
- **CD (Continuous Deployment/Delivery)** → despliega los cambios.

# Función de las dos Pipelines (CI y CD)

En Azure DevOps con Terraform y Azure Landing Zone normalmente existen dos pipelines separadas:

1. **CI (Continuous Integration)** → valida los cambios.
2. **CD (Continuous Deployment/Delivery)** → despliega los cambios.

---

# 1. Pipeline CI (Validación)

Su objetivo es responder a la pregunta:

> ¿Los cambios son correctos y pueden desplegarse?

La pipeline **NO modifica Azure**.

Simplemente revisa que todo esté bien.

## Flujo

```text
Desarrollador
    │
    ├─ Crea rama
    ├─ Hace cambios
    ├─ Commit
    └─ Push
          │
          ▼
     Pipeline CI
          │
          ├─ terraform fmt
          ├─ terraform validate
          ├─ terraform init
          ├─ terraform plan
          └─ Security Checks
```

---

## Ejemplo

Supongamos que modificas:

```text
management-groups.tf
```

Cuando haces:

```bash
git push origin feature/new-mg
```

Se ejecuta automáticamente la CI.

---

## Qué suele hacer

### Terraform Format

Comprueba formato:

```bash
terraform fmt -check
```

Ejemplo error:

```text
❌ Archivo mal formateado
```

---

### Terraform Validate

Comprueba sintaxis:

```bash
terraform validate
```

Ejemplo:

```text
❌ Variable no definida
```

---

### Terraform Plan

Calcula qué ocurriría:

```bash
terraform plan
```

Ejemplo:

```text
+ Crear Management Group
~ Modificar Policy
- Eliminar Role Assignment
```

Pero **no ejecuta nada**.

---

### Security Checks

Herramientas típicas:

```text
Checkov
TfSec
Terrascan
```

Buscan:

```text
Storage Accounts públicos
NSG demasiado abiertos
Recursos sin cifrado
```

---

## Resultado

Si todo va bien:

```text
✓ CI Passed
```

El Pull Request puede aprobarse.

---

# 2. Pipeline CD (Despliegue)

Su objetivo es responder:

> ¿Aplicamos los cambios en Azure?

Esta pipeline sí modifica Azure.

---

## Flujo

```text
Pull Request aprobado
          │
          ▼
     Merge a main
          │
          ▼
      Pipeline CD
          │
          ├─ terraform init
          ├─ terraform plan
          └─ terraform apply
```

---

## Qué hace

### Terraform Init

Configura backend:

```bash
terraform init
```

Conecta con:

```text
Storage Account
Container tfstate
```

---

### Terraform Plan

Vuelve a calcular cambios.

```bash
terraform plan
```

---

### Terraform Apply

Aplica cambios reales:

```bash
terraform apply
```

Por ejemplo:

```text
Crear Policy
Crear Management Group
Asignar Roles
Crear Resource Group
```

---

# Relación entre ambas

```text
Feature Branch
      │
      ▼
  Pipeline CI
      │
      ▼
 Pull Request
      │
      ▼
  Aprobación
      │
      ▼
 Merge a main
      │
      ▼
  Pipeline CD
      │
      ▼
 Azure
```

---

# Ejemplo práctico de Azure Landing Zone

Supongamos que añades:

```text
mg-platform
```

en Terraform.

## CI

Ejecuta:

```bash
terraform validate
terraform plan
```

Resultado:

```text
Plan:
+ Create Management Group mg-platform
```

Nadie ha creado todavía nada en Azure.

---

## CD

Tras el merge:

```bash
terraform apply
```

Resultado:

```text
Management Group creado
```

Ahora sí existe en Azure.

---

# ¿Por qué separar CI y CD?

Porque evita errores.

Sin separación:

```text
git push
      │
      ▼
terraform apply
```

Un error puede llegar directamente a Azure.

---

Con separación:

```text
git push
      │
      ▼
CI
      │
      ▼
Revisión humana
      │
      ▼
CD
```

Hay una validación técnica y una revisión funcional antes de modificar la plataforma.

---

# En vuestra Azure Landing Zone

Por lo que has mostrado anteriormente, el patrón es:

## CI

```text
terraform fmt
terraform init
terraform validate
terraform plan
```

Objetivo:

```text
Validar cambios
Generar plan
Mostrar impacto
```

---

## CD

```text
terraform init
terraform apply
```

Objetivo:

```text
Desplegar cambios en Azure
```

---

# Resumen

| Pipeline | Modifica Azure | Cuándo se ejecuta |
|-----------|-----------|-----------|
| CI | ❌ No | Push a rama o Pull Request |
| CD | ✅ Sí | Después del merge a main |
| CI | Genera Plan | Sí |
| CD | Ejecuta Apply | Sí |
| CI | Validación y calidad | Sí |
| CD | Despliegue real | Sí |

La CI responde:

```text
¿Es seguro desplegar?
```

La CD responde:

```text
Despliégalo.
```



---

# ¿Qué es un Pull Request?

Un **Pull Request (PR)** es una solicitud para integrar cambios realizados en una rama (*branch*) hacia otra rama, normalmente `main`.

Su objetivo es:

- Revisar los cambios antes de que lleguen a producción.
- Ejecutar validaciones automáticas (pipelines).
- Obtener aprobación de otros miembros del equipo.
- Mantener protegida la rama principal.

En Azure DevOps es habitual que la rama `main` tenga una política que impida hacer `push` directo.

Por eso recibiste este error:

```bash
! [remote rejected] main -> main
(TF402455: Pushes to this branch are not permitted; you must use a pull request to update this branch.)
```

Esto significa que cualquier cambio debe pasar por un Pull Request.

---

# Flujo completo de trabajo

```text
main
 │
 ├─ Crear una rama nueva
 │
 ▼
feature/nuevo-cambio
 │
 ├─ Modificar archivos
 ├─ Commit
 ├─ Push
 │
 ▼
Azure DevOps
 │
 ├─ Crear Pull Request
 ├─ Ejecutar Pipeline
 ├─ Revisión
 ├─ Aprobación
 │
 ▼
Merge a main
```

---

# Paso 1: Actualizar la rama principal

Antes de empezar, asegúrate de tener la última versión de `main`.

```bash
git checkout main
git pull
```

---

# Paso 2: Crear una rama nueva

Nunca trabajes directamente sobre `main`.

Crear una rama:

```bash
git checkout -b feature/firewall-rules
```

O con el comando moderno:

```bash
git switch -c feature/firewall-rules
```

Convenciones habituales:

```text
feature/nueva-funcionalidad
bugfix/correccion-error
hotfix/incidencia-produccion
```

Ejemplos:

```text
feature/add-firewall-rule
feature/new-policy
bugfix/fix-storage-account
```

---

# Paso 3: Realizar los cambios

Modificar los archivos necesarios.

Por ejemplo:

```text
main.tf
variables.tf
terraform.tfvars
firewall-rules.tf
```

Verificar qué ha cambiado:

```bash
git status
```

---

# Paso 4: Crear un commit

Añadir los cambios:

```bash
git add .
```

Crear el commit:

```bash
git commit -m "Add Azure Firewall rule for SAP"
```

Ejemplos de mensajes:

```text
Add Azure Firewall rule for SAP
Create management group hierarchy
Update RBAC assignments
Fix Terraform validation errors
```

---

# Paso 5: Subir la rama al repositorio

Enviar la rama a Azure DevOps:

```bash
git push origin feature/firewall-rules
```

La primera vez se crea la rama remota.

---

# Paso 6: Crear el Pull Request en Azure DevOps

Entrar en:

```text
Azure DevOps
→ Repos
→ Pull Requests
→ New Pull Request
```

Configurar:

```text
Source branch:
feature/firewall-rules

Target branch:
main
```

---

# Paso 7: Completar la información del PR

Título:

```text
Add Azure Firewall rule for SAP
```

Descripción:

```text
Adds outbound access from SAP workloads
to external service X over TCP 443.
```

También es habitual indicar:

```text
What:
- Added firewall rule

Why:
- Required by SAP integration

Impact:
- No downtime expected
```

---

# Paso 8: Revisión de cambios

Azure DevOps mostrará:

```text
Commits
Files Changed
Diffs
Comments
```

Los revisores podrán comentar líneas concretas del código.

Ejemplo:

```text
¿Podemos restringir esta regla a una IP concreta?
```

---

# Paso 9: Ejecución automática de pipelines

Al crear el PR normalmente se ejecutan pipelines como:

```text
Terraform Validate
Terraform Format Check
Terraform Plan
Checkov Security Scan
SonarQube
```

Si alguna falla:

```text
❌ Validation Failed
```

Debes corregir el problema.

---

# Paso 10: Corregir errores si es necesario

Modificar el código.

Después:

```bash
git add .
git commit -m "Fix Terraform validation error"
git push
```

No es necesario crear otro PR.

El mismo PR se actualiza automáticamente.

---

# Paso 11: Aprobación

Dependiendo de las políticas del proyecto puede requerirse:

```text
1 revisor
2 revisores
Aprobación del equipo Cloud
Aprobación del propietario del repositorio
```

Ejemplo:

```text
✓ Approved by Cloud Team
```

---

# Paso 12: Completar el Pull Request

Cuando todo esté correcto:

```text
✓ Pipeline OK
✓ Revisiones aprobadas
```

Pulsar:

```text
Complete
```

Azure DevOps fusionará la rama con `main`.

---

# Paso 13: Limpiar la rama

Muchas organizaciones eliminan automáticamente la rama después del merge.

Si no se elimina automáticamente:

```bash
git branch -d feature/firewall-rules
```

Y opcionalmente:

```bash
git push origin --delete feature/firewall-rules
```

---

# Ejemplo completo

Supongamos que quieres modificar una política de Azure Landing Zone.

Crear rama:

```bash
git checkout main
git pull

git checkout -b feature/add-deny-public-ip-policy
```

Modificar:

```text
policies/deny-public-ip.json
```

Guardar cambios.

Commit:

```bash
git add .
git commit -m "Add deny public IP policy"
```

Subir rama:

```bash
git push origin feature/add-deny-public-ip-policy
```

Crear PR:

```text
Source:
feature/add-deny-public-ip-policy

Target:
main
```

Esperar:

```text
Terraform Validate   ✓
Terraform Plan       ✓
Security Scan        ✓
```

Aprobación:

```text
Cloud Team           ✓
```

Completar:

```text
Complete Pull Request
```

Resultado:

```text
main
 └─ contiene ya la nueva política
```

---

# Flujo típico en Azure Landing Zone

```text
1. Crear rama desde main

2. Modificar Terraform/Bicep/Policies

3. git add

4. git commit

5. git push

6. Crear Pull Request

7. Terraform Validate

8. Terraform Plan

9. Revisión técnica

10. Aprobación

11. Merge a main

12. Pipeline de despliegue (Apply)
```

Este es el flujo estándar que encontrarás en la mayoría de repositorios de Azure DevOps con Terraform, Azure Landing Zones y entornos corporativos donde `main` está protegida.
