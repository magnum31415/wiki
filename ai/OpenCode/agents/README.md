# Agentes y subagentes en OpenCode

[← Índice y plantillas de OpenCode](../README.md)

## Agentes personalizados

Un agente personalizado es un asistente de IA configurado para una función específica, con instrucciones, permisos y, opcionalmente, un modelo propios. Puede ser, por ejemplo, un revisor de código o un especialista en implementación.

Para crear uno en un proyecto:

1. Crea `.opencode/agents/<nombre>.md`.
2. Añade un bloque YAML con `description` y `mode`; usa `primary` para un agente principal o `subagent` para un subagente. Añade permisos solo cuando sean necesarios.
3. Escribe las instrucciones del agente debajo del bloque YAML.
4. Para elegir un agente principal, abre `/agents` en la TUI de OpenCode. El archivo debe estar en una ruta que OpenCode descubra.

Ejemplo mínimo de agente principal:

```md
---
description: Analiza y planifica cambios sin editar archivos del proyecto.
mode: primary
---

Analiza la petición, identifica las áreas afectadas y presenta un plan.
No modifiques archivos.
```

Los agentes pueden guardarse también globalmente en `~/.config/opencode/agents/<nombre>.md`. El nombre del archivo se convierte en el ID del agente.

## Arnés de agente (agent harness) y guardrails

Un **arnés de agente** (*agent harness*) es el entorno que rodea al modelo y hace que pueda trabajar de forma acotada y comprobable en un proyecto. No es una única plantilla ni una opción concreta de OpenCode: combina identidad e instrucciones, contexto del proyecto, herramientas disponibles, permisos, flujo de trabajo y mecanismos de revisión.

Un **guardrail** es una protección o límite dentro de ese entorno. Por tanto, no son sinónimos: el arnés es el sistema completo; los guardrails son algunas de las reglas y controles que contiene. En estas plantillas hay dos clases:

- **Técnicos:** permisos que permiten, preguntan o deniegan herramientas y acciones. Son el límite más fuerte que se define aquí.
- **De proceso:** instrucciones como “trabaja en una tarea”, “escribe primero las pruebas”, “espera aprobación” o “no edites archivos”. Orientan el comportamiento, pero una instrucción en el prompt no sustituye una restricción técnica de permisos.

### Cómo lo implementan estas plantillas

- **Rol e instrucciones:** cada archivo de agente declara `description` y `mode` (`primary` o `subagent`) y define su función en el cuerpo del Markdown.
- **Permisos por responsabilidad:**
  - [Coordinator](../templates/agents/coordinator.md) no puede editar ni ejecutar shell; solo puede delegar en `planner`, `implementer` y `reviewer`.
  - [Planner](../templates/agents/planner.md) puede editar únicamente `specs/**`; no puede usar shell ni lanzar subagentes.
  - [Implementer](../templates/agents/implementer.md) puede editar y ejecutar comandos para implementar; se le instruye a completar una sola tarea, ejecutar `node --test` y detenerse. Sus permisos de edición y shell son amplios, así que conviene restringirlos más si el proyecto necesita límites técnicos más estrictos.
  - [Reviewer](../templates/agents/reviewer.md) no puede editar; el shell pide autorización por defecto y permite explícitamente `node --test`, `git diff` y `git status`.
 - **Contexto y método:** la [skill SDD](../templates/skills/sdd/SKILL.md) contiene el flujo y sus reglas; las [plantillas de comandos SDD](../sdd/README.md#automatizacion-de-sdd-mediante-skills-y-comandos) ofrecen tareas guiadas. `AGENTS.md`, `MEMORY.md` y `specs/` aportan contexto y decisiones del proyecto.
- **Comprobación y supervisión:** las plantillas piden pruebas, validación RF por RF, informes de resultados y aprobación humana entre fases. Chrome DevTools y otras herramientas externas solo están disponibles si el proyecto las configura y concede acceso.

El arnés SDD, por tanto, es la combinación de estos agentes, permisos, skill, comandos, archivos de contexto y ciclo de verificación. Los límites escritos en instrucciones son guardrails de proceso; para impedir de verdad una acción, usa permisos explícitos y comprueba que la configuración se aplica como esperas.

## Agentes incluidos en OpenCode

| Agente | Modo | Uso |
|---|---|---|
| `build` | Principal | Agente de programación predeterminado; puede modificar el proyecto. |
| `plan` | Principal | Explora y prepara planes, sin editar archivos normales del proyecto. |
| `general` | Subagente | Investigación y tareas de varios pasos; tiene acceso amplio a herramientas, pero no lanza otros subagentes. |
| `explore` | Subagente | Busca y lee código o fuentes web; no modifica archivos. |

Usa `/agents` para ver y seleccionar agentes principales en la TUI. En la documentación actual, los subagentes se ejecutan en sesiones hijas mediante la herramienta `subagent`: el agente principal los elige según la tarea y los permisos configurados.

> **Nota sobre la sintaxis:** `@` en la TUI sirve para buscar y adjuntar archivos al contexto; no es el mecanismo documentado para ejecutar un agente. Para pedir una delegación, indica al agente principal qué subagente debe usar, por ejemplo: “Pide al subagente `explore` que localice dónde se calcula la racha”.

## Subagentes

Un subagente es un agente especializado al que el agente principal delega una tarea. Se ejecuta en una sesión hija con contexto propio y devuelve informes al agente principal. El principal debe transmitirle la petición, los archivos relevantes, las restricciones y el resultado esperado; no conviene asumir que el subagente ve toda la conversación original.

Para crear uno:

1. Crea `.opencode/agents/<nombre>.md`.
2. Configura `mode: subagent`, una `description` concreta y los permisos mínimos necesarios.
3. Incluye instrucciones acotadas, el formato de respuesta esperado y qué hacer si no puede completar la tarea.
4. El agente principal puede delegarle trabajo si tiene permiso `subagent` para ese ID.

Los subagentes se ejecutan en segundo plano y, por defecto, no anidan otras delegaciones. Las reglas `subagent` del agente principal controlan qué subagentes puede lanzar; el subagente usa sus propios permisos para sus herramientas.

### Ejemplo de uso

En vez de invocar directamente `@explore`, pide al agente principal:

```text
Pide al subagente explore que investigue dónde y cómo se calcula la racha en este proyecto. Devuelve las rutas y los hallazgos relevantes, sin modificar archivos.
```

## Sistemas multiagente

Un sistema multiagente coordina varios agentes especializados para completar una tarea o un proyecto. Pueden trabajar por turnos o, si las tareas son independientes, en paralelo. Un agente coordinador, un flujo de trabajo o una persona organiza el trabajo y comparte el contexto necesario.

Un flujo posible es:

```text
coordinador → planificador → implementador → revisor
```

- **Coordinador (agente principal):** organiza las fases, delega, comunica resultados y pide aprobación cuando corresponda.
- **Planificador (subagente):** analiza la petición y prepara la especificación, el plan y las tareas; no implementa código.
- **Implementador (subagente):** ejecuta una tarea aprobada, escribe pruebas y modifica el código.
- **Revisor (subagente):** comprueba la especificación, los cambios y las pruebas sin editar archivos.

La separación de funciones no sustituye la revisión humana ni las aprobaciones definidas por el proyecto. Limita los permisos de cada agente y especifica qué fase puede realizar.

## Utilización del sistema multiagente

Con el agente `coordinator` (Coordinator) seleccionado desde `/agents`, puedes pedir un flujo completo:

```text
Quiero añadir un objetivo semanal de estudio: poder fijar cuántos minutos
quiero estudiar cada semana y ver cuánto llevo, para motivarme a cumplirlo.
Sigue el flujo SDD completo.
```

El coordinador irá delegando las fases en los subagentes. Estos trabajan en sesiones hijas y devuelven sus resultados al coordinador; la interfaz permite seguir la actividad de esas sesiones. En la TUI, la flecha hacia abajo puede servir para inspeccionar el trabajo de los subagentes, según la versión y la vista disponibles. Cuando terminan, el control vuelve al coordinador.

También se pueden delegar tareas independientes en paralelo. Por ejemplo, con `coordinator` seleccionado, pídele:

```text
Sobre `specs/002-<nombre>/spec.md` (sustituye `<nombre>` por el nombre real),
lanza en paralelo tres revisiones independientes con el subagente reviewer.
Pasa a cada uno la spec y el contexto necesario, con uno de estos focos:
1. Constitución y reglas de fechas (skill local-dates).
2. Interfaz: accesibilidad y vista móvil con Chrome DevTools.
3. Tests: qué RF están cubiertos por node --test y cuáles no.

Cuando terminen los tres, combina sus resultados en un único informe, elimina
duplicados y da un veredicto final. No modifiques archivos.
```

No uses `@reviewer` para iniciar la delegación: pide al agente principal que llame al subagente `reviewer`. El coordinador debe enviar a cada sesión hija el contexto completo y esperar los tres resultados antes de consolidarlos.

## Estructura de ejemplo

```text
project/
├── AGENTS.md
├── MEMORY.md
├── docs/
├── specs/
├── tests/
├── .agents/
├── .opencode/
│   ├── agents/
│   │   ├── coordinator.md
│   │   ├── planner.md
│   │   ├── implementer.md
│   │   └── reviewer.md
│   └── commands/
└── <código del proyecto>
```

`.agents/` puede contener instrucciones o skills de otros formatos; los perfiles de agente de OpenCode de este ejemplo se guardan en `.opencode/agents/`.

## Plantillas de flujo SDD multiagente

Estas plantillas están inactivas en este vault. Para usarlas, copia cada archivo de `../templates/agents/` a `.opencode/agents/` en el proyecto destino conservando el nombre.

| Perfil | Modo | Responsabilidad |
|---|---|---|
| [Coordinador](../templates/agents/coordinator.md) | `primary` | Conduce las fases SDD y delega en los otros tres perfiles. |
| [Planificador](../templates/agents/planner.md) | `subagent` | Redacta y mantiene artefactos dentro de `specs/`, sin tocar código. |
| [Implementador](../templates/agents/implementer.md) | `subagent` | Implementa una tarea aprobada cada vez, con pruebas primero. |
| [Revisor](../templates/agents/reviewer.md) | `subagent` | Detecta problemas y valida los RF sin modificar archivos. |

Los perfiles están redactados como ejemplo para un proyecto con flujo SDD. Ajusta el nombre del proyecto, las reglas, las rutas y los permisos antes de activarlos.

## El siguiente nivel

- **Loop engineering:** diseñar ciclos en los que un agente ejecuta, comprueba y corrige su trabajo hasta cumplir un objetivo o alcanzar un límite.
- **Graph engineering:** organizar agentes y tareas como un grafo de conexiones, dependencias y decisiones, definiendo cómo se coordinan y comparten información.
- **AgentOps:** supervisar el funcionamiento de los agentes mediante registros, métricas y seguimiento de sus acciones para detectar fallos y mantener su fiabilidad.
- **Software factory:** sistema que coordina agentes, herramientas y validaciones para convertir especificaciones en software mediante un proceso automatizado y repetible, desde la planificación hasta las pruebas y la entrega. Las personas definen los objetivos, las restricciones y los criterios de aceptación; la fábrica organiza la implementación, comprueba resultados y gestiona correcciones con distintos grados de supervisión humana.

## Las claves

- De los fundamentos → al agente con arnés → a la orquestación multiagente con specs.
- Ganan valor los fundamentos, los requisitos, el diseño y la supervisión y evaluación del código y del proyecto; pierde peso la sintaxis pura.
- **Regla de oro:** la IA ejecuta, pero el responsable del proyecto eres tú.

> **Criterio:** la potencia sin control no sirve de nada.

## Referencias

- [OpenCode — Agentes](https://opencode.ai/v2/docs/agents)
- [OpenCode — TUI y comando `/agents`](https://opencode.ai/v2/docs/cli/tui)
- [OpenCode — Herramienta `subagent`](https://opencode.ai/v2/docs/tools#subagent)
