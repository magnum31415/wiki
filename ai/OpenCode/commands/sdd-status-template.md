---
description: SDD · Informa de la fase actual y el siguiente paso de una spec.
agent: plan
---

Lee `AGENTS.md`, `MEMORY.md` si existe y los archivos disponibles en `specs/$1/` (`spec.md`, `plan.md` y `tasks.md`). Usa la skill `@sdd`.

Dime en pocas líneas:

1. En qué fase del flujo SDD está esta spec y cuál es su estado.
2. Cuántas tareas están hechas y pendientes (x de y), si existe `tasks.md`.
3. El siguiente paso exacto y el comando `/sdd-*` que debo ejecutar.

No modifiques ningún archivo. Si falta información para determinar el estado, indícalo; no la supongas.
