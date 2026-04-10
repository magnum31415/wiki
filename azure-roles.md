
[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# 📚 roles en Azure

## Planes





| Sistema                       | Qué controla                                  | Rol “admin total” recomendado                     | Notas clave                                                                 |
|------------------------------|-----------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------|
| **Entra ID (Identity)**      | Identidades, autenticación, PIM               | Global Administrator                             | Máximo privilegio en identidad. Gestiona usuarios, grupos, roles y PIM      |
| **RBAC (Control plane)**     | Recursos Azure (ARM)                          | Owner (a nivel MG o Subscription)                | Control total de recursos + puede asignar roles                             |
| **Data Plane**               | Acceso a datos (KV, Storage, SQL, etc.)       | Depende del servicio (ej: Key Vault Administrator, Storage Blob Data Owner) | No hay rol único global. Se asigna por servicio                             |
| **Billing (Cost plane)**     | Costes, facturación, usage, invoices          | Billing Account Owner (o Billing Profile Owner)  | Control total de facturación. Separado completamente de RBAC                |

##  Identity Plane (EntraId)

| Scope / Plano        | Rol                          | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|---------------------|------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Identity (Entra ID) | Global Administrator          | Control total del tenant (usuarios, grupos, roles, PIM, configuración)        | Administración central de identidad                                     | Equipo de seguridad gestionando todo el acceso corporativo                              |
| Identity (Entra ID) | Privileged Role Administrator | Gestión de roles privilegiados y PIM                                          | Control de accesos críticos                                             | Equipo IAM asignando roles elevados bajo aprobación                                     |
| Identity (Entra ID) | User Administrator            | Gestión de usuarios y grupos                                                  | Operación diaria de identidades                                         | IT creando usuarios y asignándolos a grupos                                              |
| Identity (Entra ID) | Global Reader                 | Acceso de solo lectura a toda la configuración                                | Auditoría / troubleshooting                                             | Auditor revisando configuración sin modificar                                            |

##  Control Plane (RBAC)
| Scope / Plano         | Rol                      | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|----------------------|--------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| RBAC (Control Plane) | Owner                    | Control total de recursos + asignación de roles                               | Administración completa de subscriptions / MG                          | Arquitecto cloud gestionando recursos y accesos                                          |
| RBAC (Control Plane) | Contributor              | Gestión de recursos sin poder asignar permisos                                | Operación de infraestructura                                            | Ingeniero desplegando recursos                                                           |
| RBAC (Control Plane) | Reader                   | Acceso de solo lectura a recursos                                             | Observabilidad / auditoría                                              | Equipo viendo configuración sin modificar                                                |
| RBAC (Control Plane) | User Access Administrator| Gestión de permisos RBAC                                                      | Gobierno de accesos                                                     | Equipo IAM asignando roles a usuarios o grupos                                          |

## Data Plane

| Scope / Plano | Rol                           | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|--------------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Data Plane   | Key Vault Administrator        | Control total sobre secretos, claves y certificados                           | Gestión de seguridad y secretos                                         | Equipo de seguridad gestionando secretos en Key Vault                                   |
| Data Plane   | Key Vault Secrets User         | Acceso a leer/escribir secretos                                               | Consumo de secretos por apps                                            | Aplicación accediendo a credenciales                                                     |
| Data Plane   | Storage Blob Data Owner        | Control total sobre datos en blobs                                            | Gestión de almacenamiento                                               | Equipo gestionando almacenamiento de datos                                               |
| Data Plane   | Storage Blob Data Reader       | Lectura de datos en blobs                                                     | Acceso de solo lectura                                                  | Analista leyendo datos                                                                   |
| Data Plane   | SQL DB Contributor / DB roles  | Gestión o acceso a bases de datos                                             | Administración o uso de bases de datos                                  | DBA gestionando DB o aplicación accediendo a tablas                                      |

## Cost Plane (Billing)

| Scope                | Rol                          | Para qué sirve                                                                 | Cuándo se usa                                                           | Ejemplo                                                                                  |
|----------------------|------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Billing Account      | Billing Account Owner        | Control total del contrato de facturación. Puede gestionar perfiles y acceso | Nivel corporativo / Finanzas centrales                                 | Equipo FinOps global que gestiona toda la facturación de SYNLAB                         |
| Billing Account      | Billing Account Reader       | Solo lectura del Billing Account (visión global de costes y estructura)       | Auditoría / reporting global                                           | Auditor que necesita ver el gasto total de la compañía                                  |
| Billing Profile      | Billing Profile Owner        | Control total del perfil de facturación (facturas, pagos, acceso)             | Responsable financiero de una región o unidad                          | Responsable financiero de Alemania gestionando su facturación                           |
| Billing Profile      | Billing Profile Contributor  | Puede gestionar costes (budgets, análisis), pero no todo el control total     | FinOps / Cloud team operativo                                          | Ingeniero que crea budgets y analiza costes en UK                                        |
| Billing Profile      | Billing Profile Reader       | Solo lectura de costes dentro del perfil                                      | Reporting / equipos técnicos                                           | Equipo de plataforma que revisa costes sin poder modificarlos                           |
| Invoice Section      | Invoice Section Owner        | Control total de una sección de factura (subdivisión de costes)               | Organización por departamentos / proyectos                             | Responsable de IT dentro de Alemania con control sobre su parte del gasto               |
| Invoice Section      | Invoice Section Contributor  | Gestión operativa de costes en una sección (budgets, análisis)                | Equipos de aplicación / proyectos                                      | Equipo de una aplicación que gestiona su presupuesto y consumo                          |
| Invoice Section      | Invoice Section Reader       | Solo lectura de costes en una sección concreta                                | Visibilidad para equipos                                               | Equipo de desarrollo que solo necesita ver cuánto consume su aplicación                 |
