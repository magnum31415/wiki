#

## Estructura

En Claude Code, el fichero estándar para instrucciones del proyecto es CLAUDE.md

````
my-project/
├── CLAUDE.md
├── specs/
│   ├── PRODUCT.md
│   ├── ARCHITECTURE.md
│   ├── REQUIREMENTS.md
│   └── CHANGELOG.md
├── src/
├── tests/
└── README.md
````

## Contenido de cada fichero

- ``CLAUDE.md `` → reglas para Claude.
  - Debe ser relativamente estable:
    - tecnologías
    - convenciones
    - arquitectura
    - cómo ejecutar tests
    - qué puede/no puede modificar y
    - muy importante
    - decirle que consulte specs/ antes de implementar.
- ``PRODUCT.md`` → para explicar qué aplicación estás construyendo
- ``REQUIREMENTS.md`` → Estado deseado actual. Ahí puedes ir evolucionando las funcionalidades
- ``CHANGELOG.md `` = qué hemos cambiado
  
### Ejemplo de CLAUDE.md

````
# Project Instructions

## Specifications

Before implementing any change:

1. Read the relevant files under `/specs`.
2. Treat `specs/REQUIREMENTS.md` as the source of truth for functional requirements.
3. Treat `specs/ARCHITECTURE.md` as the source of truth for architecture.
4. Update the relevant specification when functionality changes.
5. Record significant implemented changes in `specs/CHANGELOG.md`.

## Development rules

- Do not change architecture without explicit approval.
- Do not introduce new dependencies unless necessary.
- Follow existing coding conventions.
- Add or update tests for every functional change.
- Do not remove existing functionality unless explicitly requested.
- Prefer modifying existing components over duplicating functionality.
- Run tests before considering a task complete.

## Source of truth

The files under /specs define the expected application behavior.

If the implementation conflicts with the specs, do not silently choose one.
Explain the conflict before changing the implementation.
````

### Ejemplo de PRODUCT.md

````
# Product

## Purpose

Application for managing ...

## Users

- Administrator
- Standard user

## Main features

- Authentication
- User management
- ...
````

### Ejemplo de REQUIREMENTS.md

De esta forma puedes llegar a Claude Code y decir simplemente: ``Implement REQ-002 according to the specs.``

````
# Application Requirements

## REQ-001 - Authentication

Status: Implemented

Description:
Users authenticate using Microsoft Entra ID.

Acceptance criteria:
- Users can authenticate using Entra ID.
- Unauthorized users cannot access the application.


## REQ-002 - Dashboard

Status: Change requested

Description:
The application provides a dashboard with usage information.

Current requirements:
- Display usage statistics.
- Display data by country.

New requirements:
- Add filtering by country.
- Add filtering by date.
- Default period must be 30 days.

Acceptance criteria:
- User can select one or multiple countries.
- User can select a date range.
- Dashboard defaults to the last 30 days.
````
