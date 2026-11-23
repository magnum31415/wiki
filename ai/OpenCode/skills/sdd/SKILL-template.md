---
name: sdd
description: >-
  Úsala siempre que trabajes con Spec-Driven Development en este proyecto:
  redactar, revisar o cambiar specs, planes y tareas, o implementar y validar
  tareas de una spec en docs/constitution.md o dentro de specs/.
---

# Spec-Driven Development (SDD)

## Flujo

Constitución → Spec → Clarificación → Plan → Tareas → Implementación → Validación → Cambio.

## Reglas

- No pases a la siguiente fase sin la aprobación explícita del usuario.
- La spec define el comportamiento: si algo no está especificado, no lo implementes. Si falta una decisión, detente y pregunta.
- Un cambio de requisitos se incorpora primero a la spec; después se actualizan el plan y las tareas; por último, el código.
- Cada spec vive en `specs/NNN-nombre/` y contiene `spec.md`, `plan.md` y `tasks.md`.
- Antes de trabajar, lee `AGENTS.md`, `docs/constitution.md` y los archivos disponibles de la spec activa.
- Al terminar cada fase, actualiza `MEMORY.md` con el estado y las decisiones relevantes. No inventes avances ni guardes datos sensibles.

## Plantilla de spec (`spec.md`)

La spec describe el **qué** y el **por qué**. No incluyas stack, arquitectura ni nombres de archivos.

```md
# Spec NNN — <Nombre de la funcionalidad>

Estado: borrador | aprobada | implementada

## Contexto y objetivo
<Qué problema resuelve y por qué merece la pena. Un párrafo.>

## Usuarios
<Quién lo usa.>

## Historias de usuario
- HU-1. Como <rol>, quiero <acción> para <beneficio>.

## Definiciones
<Solo si hay términos que puedan interpretarse de varias formas.>

## Requisitos funcionales
<Requisitos con sintaxis EARS; cada uno debe ser verificable.>

## Requisitos no funcionales
<Solo los que apliquen: rendimiento, seguridad, plataformas, idioma, etc.>

## Casos límite
<Vacíos, duplicados, datos corruptos, límites, concurrencia, etc.>

## Fuera de alcance
<Lo que explícitamente NO se hace en esta iteración.>

## Criterios de finalización
<Comprobaciones necesarias para considerar la spec cumplida.>

## Dudas abiertas
- [NECESITA ACLARACIÓN] <duda>
```

## Requisitos en EARS (en español)

- `RF-x: CUANDO <evento>, EL SISTEMA <respuesta>.`
- `RF-x: SI <condición no deseada>, ENTONCES EL SISTEMA <respuesta>.`
- `RF-x: MIENTRAS <estado>, EL SISTEMA <respuesta>.`
- `RF-x: EL SISTEMA <comportamiento permanente>.`

Cada requisito funcional debe poder verificarse. Evita términos subjetivos como “rápido”, “intenso” o “bonito” si no tienen un criterio medible.

## Plan (`plan.md`)

Relaciona cada parte con los RF que cubre. Incluye:

- Archivos que se crearían o modificarían y la responsabilidad de cada uno.
- Funciones puras de lógica, usando `"hoy"` como parámetro cuando corresponda.
- Algoritmos en pseudocódigo y cómo se representa la interfaz.
- Decisiones técnicas justificadas y la alternativa descartada.
- Estrategia de pruebas con `node --test`.

## Tareas (`tasks.md`)

Desglosa el plan en tareas de 20–30 minutos como máximo, ordenadas por dependencias y vinculadas a los RF:

```md
- [ ] **Tn. <Descripción>.** RF-x, RF-y
  - Hecho cuando: <comprobación verificable>.
```

Si se necesitan más de 10 tareas, propone dividir la spec antes de continuar.

## Implementación

Implementa una sola tarea cada vez. Escribe primero las pruebas, ejecuta `node --test`, completa el código hasta que pasen, marca la tarea como hecha e indica los RF cubiertos. Después, detente: no empieces la siguiente tarea sin aprobación.

## Validación

Recorre los RF uno por uno e indica qué prueba o comprobación verifica cada uno y su resultado. Para requisitos de interfaz que no se puedan comprobar con `node --test`, usa Chrome DevTools si está disponible, incluida la vista móvil. No afirmes que un requisito está cumplido si no se ha comprobado.

## Cambio de requisitos

Actualiza primero la spec y presenta el cambio para su revisión. Tras su aprobación, indica qué debe cambiar en el plan y las tareas. No modifiques código antes de actualizar y aprobar los artefactos correspondientes.
