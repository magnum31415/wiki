# Spec-Driven Development (SDD)

[← Índice y plantillas de OpenCode](../README.md)

## ¿Qué es?

Spec-Driven Development es una forma de desarrollar software en la que primero se definen el comportamiento esperado, los requisitos, los límites y los criterios de aceptación. La especificación guía la planificación, la implementación y la validación.

No consiste solo en escribir más documentación: la especificación debe ser suficientemente concreta para orientar el trabajo y comprobar si el resultado cumple lo acordado.

## Taxonomía práctica de SDD

Estos términos describen distintos grados de dependencia de la especificación. No son fases obligatorias de un mismo proceso.

### 1. Spec-first

Se escribe una especificación antes de implementar. Después de generar la primera versión, el código suele evolucionar directamente y la especificación puede quedar desactualizada.

### 2. Spec-anchored

La especificación se conserva como documento vivo y se actualiza junto con el sistema. Sirve de referencia para cambios futuros. Es un buen punto de partida para adoptar SDD.

### 3. Spec-as-source

La especificación es la fuente de verdad y el código se genera a partir de ella. Los cambios se hacen primero en la especificación y luego se regenera el código; las modificaciones manuales al código generado no se consideran la fuente mantenible.

## ¿Por qué ayuda al trabajar con IA?

- Los requisitos observables y verificables se pueden relacionar con pruebas o comprobaciones de aceptación.
- Explicitar condiciones, errores y respuestas esperadas ayuda a evitar que se consideren solo los casos habituales.
- Separar los requisitos en comportamientos concretos reduce la ambigüedad y facilita revisar lo que hizo el agente.
- La relación entre requisito, tarea y verificación permite detectar requisitos que no se implementaron o comprobaron.

La especificación no garantiza por sí sola que el resultado sea correcto: hay que revisar los requisitos, ejecutar las verificaciones y mantenerla al día.

## EARS (Easy Approach to Requirements Syntax)

EARS es una forma de redactar requisitos usando patrones para hacerlos más claros y menos ambiguos. Cada requisito debería describir un comportamiento identificable y comprobable.

Algunos patrones habituales:

- **Siempre activo:** `El sistema deberá <comportamiento observable>.`
- **Evento:** `CUANDO <ocurra un evento>, el sistema deberá <respuesta>.`
- **Estado:** `MIENTRAS <se cumpla un estado>, el sistema deberá <respuesta>.`
- **Condición o comportamiento no deseado:** `SI <se cumple una condición>, ENTONCES el sistema deberá <respuesta>.`
- **Funcionalidad opcional:** `DONDE <la funcionalidad esté habilitada>, el sistema deberá <respuesta>.`

El patrón `SI ... ENTONCES` es útil para explicitar errores y condiciones, pero no es el único patrón de EARS. Elige el que corresponda y evita juntar varios comportamientos distintos en una sola frase.

## Estructura de un proyecto

```text
project/
├── .opencode/                 # Configuración de OpenCode, si se utiliza
├── .agents/                   # Configuración de otros agentes, si se utiliza
├── AGENTS.md                  # Instrucciones permanentes para el agente
├── MEMORY.md                  # Memoria entre sesiones, si se utiliza
├── docs/
│   └── constitution.md        # Principios y límites del proyecto
├── specs/
│   ├── 001-nombre-spec/
│   │   ├── spec.md            # Requisitos y criterios de aceptación
│   │   ├── plan.md            # Enfoque de implementación
│   │   └── tasks.md           # Trabajo desglosado y verificable
│   └── 002-otra-spec/
├── tests/
└── <código del proyecto>
```

La constitución describe reglas estables del proyecto. Cada funcionalidad o cambio de alcance tiene su propia carpeta de especificación. `AGENTS.md` contiene instrucciones para el agente; no sustituye las especificaciones funcionales.

## Flujo SDD paso a paso

1. **Constitución — una vez por proyecto.** Define el objetivo, principios, límites y restricciones duraderas en `docs/constitution.md`.
2. **Especificación — por funcionalidad.** Describe el problema, el alcance, los requisitos y los criterios de aceptación en `spec.md`.
3. **Clarificación.** Resuelve ambigüedades, contradicciones, casos límite y decisiones que requieren la participación de una persona. Anota los supuestos pendientes.
4. **Planificación.** Explica la solución propuesta, las áreas afectadas, las dependencias, los riesgos y cómo se comprobará el resultado en `plan.md`.
5. **Tareas.** Divide el plan en tareas pequeñas, ordenadas y relacionadas con los requisitos en `tasks.md`.
6. **Implementación.** Ejecuta las tareas respetando la constitución y la especificación. Si es necesario cambiar el comportamiento acordado, actualiza primero la especificación o registra la decisión.
7. **Validación.** Ejecuta las pruebas y comprobaciones, y contrasta cada criterio de aceptación con el resultado.

## Automatizacion-de-SDD-mediante-Skills-y-Comandos

En OpenCode, las **skills** se invocan con `@<id>` y los **comandos** con `/nombre`. Por eso, la skill general de SDD se invocaría como `@sdd`, no como `/sdd`.

**Skill:** [`@sdd`](../templates/skills/sdd/SKILL.md) — guía el flujo SDD y el uso de las plantillas de especificación.

| Comando | Ejemplo de uso | Fase | Agente |
|---|---|---|---|
| [`/sdd-constitution`](../templates/commands/sdd-constitution.md) | `/sdd-constitution` | Constitución | `build` |
| [`/sdd-spec`](../templates/commands/sdd-spec.md) | `/sdd-spec 002-nombre idea…` | Especificación | `build` |
| [`/sdd-clarify`](../templates/commands/sdd-clarify.md) | `/sdd-clarify 002-nombre` | Clarificación | `plan` |
| [`/sdd-plan`](../templates/commands/sdd-plan.md) | `/sdd-plan 002-nombre` | Planificación | `build` |
| [`/sdd-tasks`](../templates/commands/sdd-tasks.md) | `/sdd-tasks 002-nombre` | Tareas | `build` |
| [`/sdd-implement`](../templates/commands/sdd-implement.md) | `/sdd-implement 002-nombre T3` | Implementación | `build` |
| [`/sdd-validate`](../templates/commands/sdd-validate.md) | `/sdd-validate 002-nombre` | Validación | `build` |
| [`/sdd-change`](../templates/commands/sdd-change.md) | `/sdd-change 002-nombre requisito…` | Mantenimiento | `build` |
| [`/sdd-status`](../templates/commands/sdd-status.md) | `/sdd-status 002-nombre` | Fase actual y siguiente paso de una spec | `plan` |

Estos archivos son plantillas y no están activos en este vault. Para usar la skill, copia `../templates/skills/sdd/SKILL.md` al proyecto como `.agents/skills/sdd/SKILL.md` (OpenCode también admite `.opencode/skills/sdd/SKILL.md`). Para activar un comando, copia su plantilla a `.opencode/commands/` conservando el nombre.

Los comandos que escriben artefactos usan `build`; los de solo lectura usan `plan`. El agente `plan` integrado no puede editar archivos normales del proyecto.

### Prompts de ejemplo para `001-heat-map`

Adapta los identificadores, las rutas y los comandos al proyecto. Estos prompts ilustran cómo pedir cada etapa al agente.

#### Paso 3: Clarificación

**Prompt para clarificar una especificación:**

```text
Revisa specs/001-heat-map/spec.md como si fueras un QA muy profesional.
Lista: (1) ambigüedades restantes, (2) contradicciones entre requisitos,
(3) casos límite no cubiertos, (4) conflictos con docs/constitution.md.
No propongas soluciones todavía: solo detecta. Formato: lista numerada.
```

#### Paso 4: Planificación (`plan.md`)

**Prompt para crear la planificación de la especificación:**

```text
Lee docs/constitution.md y specs/001-heat-map/spec.md. NO escribas
código. Genera specs/001-heat-map/plan.md con: qué archivos se crean o
modifican y qué responsabilidad tiene cada uno, qué funciones puras de lógica
se necesitan (con "hoy" como parámetro), algoritmo del mapa en pseudocódigo,
cómo se pinta en la interfaz, decisiones técnicas justificadas (y su
alternativa descartada) y estrategia de tests con node --test. Todo debe
respetar la constitución y cubrir todos los RF. Marca qué RF cubre cada parte.
```

#### Paso 5: Tareas (`tasks.md`)

**Prompt para crear las tareas a partir de la planificación:**

```text
A partir de spec.md y plan.md, genera specs/001-heat-map/tasks.md:
tareas pequeñas (máx. 20-30 min cada una), en orden de dependencia, cada una
con los RF que cubre y una línea "Hecho cuando:" verificable. Usa checkboxes.
```

#### Paso 6: Implementación

**Prompt para implementar una tarea:**

```text
Implementa SOLO la tarea T1 de specs/001-heat-map/tasks.md, siguiendo
plan.md y la constitución. Escribe primero los tests, luego el código.
Ejecuta node --test y muéstrame el resultado. Al terminar: marca T1 en
tasks.md, indica qué RF cubre y PÁRATE. No empieces T2.
```

#### Paso 7: Validación

**Prompt para validar la especificación implementada:**

```text
Recorre specs/001-heat-map/spec.md requisito por requisito. Para cada
uno indica qué test lo cubre y el resultado de ejecutarlo. Los RF de interfaz
que no se puedan testear con node --test, verifícalos con Chrome DevTools
(incluida la vista móvil). Si algún RF no está cubierto o falla, dilo
claramente. Después comprueba los criterios de finalización y dame un
veredicto: ¿la spec está cumplida?
```

> La clarificación se puede repetir después de los pasos 4 y 5 si aparecen dudas o inconsistencias.

**Mantenimiento:** para una evolución futura, vuelve al paso 2. Actualiza la especificación y repite la clarificación, planificación, tareas, implementación y validación que sean necesarias.

## Usar las plantillas

- Copia [`templates/sdd/docs/constitution.md`](../templates/sdd/docs/constitution.md) al proyecto como `docs/constitution.md`.
- Copia la [plantilla de la skill SDD](../templates/skills/sdd/SKILL.md) al proyecto como `.agents/skills/sdd/SKILL.md`.
- Copia las plantillas de comandos enlazadas en la tabla a `.opencode/commands/` conservando sus nombres para activarlas.
- Copia la carpeta `templates/sdd/specs/001-spec-name/` al proyecto y adáptala al ID y nombre de la funcionalidad, por ejemplo `specs/001-autenticacion/`.
- Dentro de la carpeta copiada, adapta los archivos [`spec.md`](../templates/sdd/specs/001-spec-name/spec.md), [`plan.md`](../templates/sdd/specs/001-spec-name/plan.md) y [`tasks.md`](../templates/sdd/specs/001-spec-name/tasks.md).
- Sustituye los marcadores y elimina los apartados que no apliquen. Mantén las decisiones y requisitos basados en información confirmada.

## Referencias

- [Easy Approach to Requirements Syntax (EARS), artículo original en IEEE](https://ieeexplore.ieee.org/document/5328509)
- [Spec Kit — guía de inicio de SDD](https://github.github.com/spec-kit/quickstart.html)
- [Spec-Driven Development: From Code to Contract (arXiv)](https://arxiv.org/html/2602.00180v1)
