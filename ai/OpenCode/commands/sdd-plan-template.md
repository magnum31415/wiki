---
description: SDD · Genera el plan técnico de una spec aprobada.
agent: build
---

Lee `AGENTS.md`, `docs/constitution.md`, `specs/$1/spec.md` y `MEMORY.md` si existe. Usa la skill `@sdd`.

No escribas código. Si la spec no está aprobada o tiene dudas abiertas, detente y avísame.

Genera `specs/$1/plan.md` con:

- Los archivos que se crean o modifican y la responsabilidad de cada uno.
- Las funciones puras de lógica necesarias, usando `"hoy"` como parámetro cuando corresponda.
- El algoritmo en pseudocódigo.
- Cómo se pinta en la interfaz.
- Decisiones técnicas justificadas y su alternativa descartada.
- La estrategia de pruebas con `node --test`.

Todo debe respetar la constitución y cubrir todos los RF. Indica qué RF cubre cada parte. No modifiques ningún otro archivo ni avances a tareas hasta que apruebe el plan.
