---
description: SDD · Incorpora un requisito nuevo primero en la spec.
agent: build
---

Nuevo requisito para `specs/$1/` (el texto que va después del nombre de la carpeta): $2

No toques código. Lee `AGENTS.md`, `docs/constitution.md`, `MEMORY.md` si existe y usa la skill `@sdd`.

1. Actualiza solo `specs/$1/spec.md`: añade o modifica el RF en EARS, sus casos límite y lo que queda fuera de alcance.
2. Indica qué partes de `plan.md` y `tasks.md` habría que cambiar después.
3. Muéstrame el diff de la spec y espera mi aprobación antes de modificar el plan, las tareas o el código.
