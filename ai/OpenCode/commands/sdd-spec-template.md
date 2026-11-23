---
description: >-
  SDD · Entrevista y prepara la spec.
  Uso: /sdd-spec 002-nombre idea inicial
agent: build
---

No escribas código en ningún momento. Lee `AGENTS.md`, `docs/constitution.md` y `MEMORY.md` si existe. Usa la skill `@sdd`.

Carpeta de la spec: `specs/$1/`
Idea inicial (el texto que va después del nombre de la carpeta): $2

Tu trabajo:

1. Hazme preguntas de una en una para eliminar ambigüedades: casos límite, errores y qué queda fuera de esta versión. Haz como máximo 5 preguntas.
2. Con mis respuestas, prepara `specs/$1/spec.md` siguiendo la plantilla de la skill y con `Estado: borrador`.
3. La spec debe describir solo el qué y el porqué: nada de stack, arquitectura ni nombres de archivos.
4. Presenta la spec y espera mi aprobación antes de pasar a planificación. No empieces `plan.md`.
