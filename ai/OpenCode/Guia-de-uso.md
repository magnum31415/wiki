# Guía de uso de las plantillas de OpenCode

Esta guía explica cómo preparar las plantillas de `templates/` para un proyecto y utilizarlas durante el desarrollo con IA. Esa carpeta no es una ruta de descubrimiento por defecto para agentes, comandos o skills; copia los archivos a las ubicaciones del proyecto indicadas aquí. **Excepción:** OpenCode puede cargar `templates/AGENTS.md` como instrucción al trabajar dentro de esa carpeta o explorarla.

## 1. Prepara el contexto del proyecto

En la raíz del proyecto:

- Copia `templates/AGENTS.md` a la raíz del proyecto y completa las instrucciones permanentes: objetivo, stack, estructura, convenciones y comandos comprobados.
- Copia `templates/MEMORY.md` como `MEMORY.md` si quieres mantener entre sesiones las decisiones y el estado del trabajo. Es opcional; no incluyas secretos ni datos personales.
- Crea `docs/` y `specs/` si todavía no existen.

No dejes marcadores de plantilla sin resolver. Asegúrate de que las instrucciones reflejen el repositorio real.

## 2. Instala el flujo SDD

Para el flujo SDD con coordinador y subagentes, copia las plantillas así:

- `templates/sdd/docs/constitution.md` → `docs/constitution.md`.
- `templates/skills/sdd/SKILL.md` → `.agents/skills/sdd/SKILL.md`.
- Copia los cuatro perfiles de `templates/agents/` a `.opencode/agents/` con sus nombres: `coordinator.md`, `planner.md`, `implementer.md` y `reviewer.md`.
- Copia los comandos `templates/commands/sdd-*.md` a `.opencode/commands/` con los mismos nombres, por ejemplo `sdd-spec.md`.
- Copia `templates/sdd/specs/001-spec-name/` a `specs/NNN-nombre/` para cada funcionalidad; sus archivos ya se llaman `spec.md`, `plan.md` y `tasks.md`.

Si no quieres usar un sistema multiagente, puedes empezar con la skill SDD y los comandos; los perfiles `coordinator`, `planner`, `implementer` y `reviewer` son opcionales. Para cambios pequeños fuera del flujo SDD, puedes instalar también `templates/commands/feature.md` como `.opencode/commands/feature.md`.

## 3. Adapta instrucciones, pruebas y permisos

Antes de activar los archivos:

- Sustituye rutas, nombres y reglas de ejemplo por los del proyecto.
- Las plantillas usan `node --test` como estrategia de pruebas. Si el proyecto usa otro lenguaje o runner, adapta la skill, los comandos y los perfiles para ejecutar el comando de pruebas real y comprobado.
- Chrome DevTools solo se puede usar si el proyecto tiene configurado y autorizado ese MCP.
- Revisa los permisos de los agentes. En particular, `implementer` tiene permisos amplios de edición y shell; redúcelos si el proyecto necesita límites técnicos más estrictos.
- Conserva las aprobaciones entre fases. Las instrucciones orientan al agente, pero los permisos son los que restringen técnicamente sus herramientas.

## 4. Selecciona el coordinador

Abre OpenCode desde la raíz del proyecto y usa `/agents` para seleccionar `coordinator`. También puedes trabajar sin coordinador y ejecutar los comandos SDD manualmente.

Con el coordinador seleccionado, puedes describir el resultado que buscas y pedir el flujo completo. Por ejemplo:

```text
Quiero añadir un objetivo semanal de estudio: poder fijar cuántos minutos
quiero estudiar cada semana y ver cuánto llevo, para motivarme a cumplirlo.
Sigue el flujo SDD completo y espera mi aprobación antes de pasar de fase.
```

El coordinador delega tareas en los subagentes. Proporciónale una petición clara y revisa sus informes; las sesiones hijas no necesariamente reciben todo el contexto de esta conversación.

## 5. Sigue las fases SDD

El flujo recomendado es:

1. **Constitución:** ejecuta `/sdd-constitution`. Revisa y aprueba los principios antes de continuar.
2. **Especificación:** ejecuta `/sdd-spec 001-nombre <idea inicial>`. Responde las preguntas y revisa `specs/001-nombre/spec.md`, que se crea como borrador.
3. **Clarificación:** ejecuta `/sdd-clarify 001-nombre`. Resuelve las dudas y contradicciones; repite la revisión si hace falta. Aprueba la spec antes de planificar.
4. **Planificación:** ejecuta `/sdd-plan 001-nombre`. Revisa que el plan cubra los RF y respete la constitución. Apruébalo antes de generar las tareas.
5. **Tareas:** ejecuta `/sdd-tasks 001-nombre`. Comprueba dependencias, alcance y criterios verificables; aprueba la lista antes de implementar.
6. **Implementación:** ejecuta `/sdd-implement 001-nombre T1`. El agente debe escribir pruebas primero, ejecutar las pruebas, marcar solo esa tarea y detenerse. Revisa el resultado antes de pedir T2.
7. **Validación:** cuando estén implementadas las tareas, ejecuta `/sdd-validate 001-nombre`. Comprueba los RF uno por uno y revisa el veredicto; los RF de interfaz requieren comprobación visual cuando corresponda.

No avances automáticamente entre fases: revisa los artefactos y da aprobación explícita. La spec define el comportamiento acordado; si cambia el requisito, actualiza primero la spec.

## 6. Mantén el estado y gestiona cambios

- Actualiza `MEMORY.md` al terminar cada fase con el estado y las decisiones reales, si el proyecto lo utiliza.
- Para consultar la fase y el siguiente paso, usa `/sdd-status 001-nombre`.
- Para un nuevo requisito, usa `/sdd-change 001-nombre <requisito>`. Revisa y aprueba el cambio de spec antes de modificar el plan, las tareas o el código.
- Al cerrar, conserva la relación entre requisitos, tareas, pruebas y resultados. No marques como cumplido un RF que no se haya verificado.

## Responsabilidad y supervisión

El arnés organiza el trabajo y los guardrails limitan algunas acciones, pero no sustituyen tu criterio. Revisa los diffs, los resultados de pruebas y las decisiones técnicas; tú eres responsable de las aprobaciones y del resultado del proyecto.

Para más detalle, consulta la [guía SDD](sdd/README.md) y la [guía de agentes y guardrails](agents/README.md).
