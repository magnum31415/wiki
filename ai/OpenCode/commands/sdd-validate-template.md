---
description: SDD · Valida la spec requisito por requisito con pruebas y revisión de interfaz.
agent: build
---

Recorre `specs/$1/spec.md` requisito por requisito y usa la skill `@sdd`.

Para cada RF, indica qué prueba lo cubre y el resultado de ejecutarla con `node --test`.

Los RF de interfaz que no se puedan probar con `node --test`, verifícalos con el MCP de Chrome DevTools si está disponible, incluida la vista móvil de 375 px. Si no está disponible, indícalo claramente.

Si algún RF no está cubierto o falla, dilo claramente. No arregles nada todavía. Después comprueba los criterios de finalización y dame un veredicto: ¿la spec está cumplida?
