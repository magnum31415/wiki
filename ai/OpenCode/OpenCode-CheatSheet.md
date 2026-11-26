# Chuleta OpenCode — macOS (Terminal)

Atajos de teclado para OpenCode TUI.

## 1. Los imprescindibles

| Acción | Atajo |
|---|---|
| Cambiar Build ↔ Plan | `Shift + Tab` |
| Cambiar Basic / Medium / High | `Ctrl + T` |
| Seleccionar modelo de IA | `Ctrl + X` → `M` |
| Siguiente modelo reciente | `F2` |
| Modelo reciente anterior | `Shift + F2` |
| Seleccionar agente | `Ctrl + X` → `A` |
| Abrir paleta de comandos | `Ctrl + P` |

> **Nota:** Los niveles de razonamiento disponibles dependen del modelo seleccionado.

## 2. Sesiones y conversaciones

| Acción | Atajo |
|---|---|
| Nueva sesión | `Ctrl + X` → `N` |
| Listar sesiones | `Ctrl + X` → `L` |
| Siguiente pestaña | `Ctrl + Tab` |
| Pestaña anterior | `Ctrl + Shift + Tab` |
| Deshacer última interacción | `Ctrl + X` → `U` |
| Rehacer | `Ctrl + X` → `R` |
| Compactar contexto | `Ctrl + X` → `C` |
| Interrumpir respuesta | `Esc` |
| Cerrar pestaña | `Ctrl + X` → `W` |

## 3. Escribir y trabajar con archivos

| Acción | Atajo o comando |
|---|---|
| Enviar prompt | `Enter` |
| Salto de línea | `Shift + Enter` |
| Referenciar archivos | `@` |
| Ejecutar comando shell | `!` al inicio |
| Abrir editor externo | `Ctrl + X` → `E` |
| Limpiar entrada | `Ctrl + C` |
| Borrar palabra anterior | `Ctrl + W` |
| Mostrar barra lateral | `Ctrl + X` → `B` |
| Abrir/cerrar terminal | `Ctrl + X` → `T` |

## 4. Comandos útiles

| Comando | Función |
|---|---|
| `/models` | Elegir modelo |
| `/agents` | Elegir agente |
| `/sessions` | Listar sesiones |
| `/new` | Nueva conversación |
| `/undo` | Deshacer última interacción |
| `/redo` | Rehacer |
| `/editor` | Abrir editor externo |

## 5. Estructura del directorio `.opencode`

La configuración de proyecto puede estar en `opencode.json`/`opencode.jsonc` en la raíz o dentro de `.opencode/`. Este ejemplo la sitúa dentro de `.opencode`; crea solo las carpetas y archivos que vayas a usar:

```text
project/
├── AGENTS.md
├── docs/
├── specs/
├── tests/
└── .opencode/
    ├── opencode.jsonc             # también puede estar en la raíz del proyecto
    ├── agents/
    │   └── coordinator.md
    ├── commands/
    │   └── sdd-spec.md
    ├── plugins/
    │   └── local-plugin.ts
    ├── skills/
    │   └── <skill-id>/
    │       └── SKILL.md
    └── themes/
        └── project-theme.json
```

> En un proyecto sencillo, elige una ubicación para la configuración: `opencode.json(c)` en la raíz o `.opencode/opencode.json(c)`. OpenCode también puede combinar configuraciones en distintos niveles; consulta la [documentación de configuración](https://opencode.ai/v2/docs/config) si necesitas esa precedencia.

- `agents/`: perfiles de agentes personalizados.
- `commands/`: comandos Markdown invocados con `/`.
- `plugins/`: plugins locales `.js`/`.ts` o directorios de paquetes.
- `skills/`: instrucciones reutilizables en archivos `SKILL.md`, con sus referencias o scripts.
- `themes/`: temas del proyecto en archivos `.json`.

`AGENTS.md`, `docs/`, `specs/` y `tests/` pertenecen a la raíz del proyecto, no a `.opencode/`. Para ver ejemplos de SDD y agentes, consulta la [guía de uso](Guia-de-uso.md).

## 6. Cómo utilizar los atajos

### Atajos secuenciales

Cuando aparece:

`Ctrl + X` → `M`

Significa:

1. Pulsar `Ctrl + X`.
2. Soltar ambas teclas.
3. Pulsar `M`.

No hay que pulsar las tres teclas simultáneamente.

## 7. Resumen rápido

```text
Shift + Tab       Cambiar Build / Plan
Ctrl + T          Cambiar nivel de razonamiento
Ctrl + X → M      Cambiar modelo
Ctrl + X → A      Cambiar agente
Ctrl + P          Paleta de comandos
Ctrl + X → N      Nueva sesión
Ctrl + X → L      Listar sesiones
Ctrl + X → C      Compactar contexto
Ctrl + X → U      Deshacer
Esc               Interrumpir respuesta
```

## 8. Documentación

- [OpenCode — Keyboard shortcuts](https://opencode.ai/docs/cli/keybinds)
- [OpenCode — Documentación oficial](https://opencode.ai/docs/)

> Los atajos son valores predeterminados y pueden variar según la versión o la configuración personalizada de OpenCode.
