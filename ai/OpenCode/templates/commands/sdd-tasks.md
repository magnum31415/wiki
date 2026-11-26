---
description: SDD · Divide el plan aprobado en tareas pequeñas y verificables.
agent: build
---

A partir de `specs/$1/spec.md` y `specs/$1/plan.md`, usa la skill `@sdd` y genera `specs/$1/tasks.md`.

- Crea tareas pequeñas, de 20–30 minutos como máximo, en orden de dependencia.
- Indica en cada tarea los RF que cubre y una línea `Hecho cuando:` verificable.
- Usa checkboxes.
- Intenta no superar las 10 tareas. Si hacen falta más, propón dividir la spec antes de crear `tasks.md`.

No modifiques otros archivos ni empieces la implementación.
