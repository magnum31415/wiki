# Plantillas de OpenCode

Esta carpeta reúne referencias y plantillas reutilizables para proyectos que usan OpenCode.
Los archivos con el sufijo `-template` son plantillas de referencia; no son instrucciones ni comandos activos en este vault.

## Contenido

- [Chuleta de OpenCode para macOS](OpenCode-CheatSheet.md)
- [Plantilla de `AGENTS.md`](AGENTS-template.md)
- [Plantilla de `MEMORY.md`](MEMORY-template.md)
- [Plantilla del comando para planificar funcionalidades](commands/feature-template.md)
- [Plantilla de Skill](skills/SKILL-template.md)
- [Plantilla de Skill SDD](skills/sdd/SKILL-template.md)
- [Guía de uso de las plantillas en un proyecto](Guia-de-uso.md)
- [Guía SDD y plantillas](sdd/README.md)
- [Guía de agentes, subagentes y sistemas multiagente](agents/README.md)

## Empezar un proyecto con estas plantillas

Si vas a desarrollar un proyecto con IA, consulta la [guía de uso](Guia-de-uso.md): explica cómo instalar y adaptar las plantillas, seleccionar el coordinador y recorrer el flujo SDD con aprobaciones humanas. El README sirve como índice; la guía reúne el procedimiento paso a paso.

## Agentes y subagentes

Los agentes personalizados permiten separar responsabilidades mediante instrucciones, modos de ejecución y permisos; los subagentes trabajan en sesiones independientes y devuelven sus resultados al agente principal. Consulta la [guía de agentes y sistemas multiagente](agents/README.md), con ejemplos, plantillas y una explicación del arnés de agente y sus guardrails.

## Spec-Driven Development (SDD)

SDD define primero qué debe hacer el sistema: requisitos, límites y criterios de aceptación. Esa especificación guía la planificación, la implementación y la validación.

### Estilos de SDD

- **Spec-first:** la especificación sirve de punto de partida; después, el código suele evolucionar directamente y la spec puede quedar desactualizada.
- **Spec-anchored:** la especificación se mantiene viva y se actualiza junto con el código. Es un buen enfoque para empezar.
- **Spec-as-source:** la especificación es la fuente de verdad y el código se genera a partir de ella; los cambios se hacen en la spec y se vuelve a generar el código.

### Requisitos con EARS

EARS (*Easy Approach to Requirements Syntax*) usa patrones para redactar requisitos claros y verificables. Por ejemplo: `CUANDO <evento>, el sistema deberá <respuesta>` o `SI <condición>, ENTONCES el sistema deberá <respuesta>`. Conviene describir un comportamiento por requisito y explicitar los casos límite.

### Flujo habitual

1. Definir la constitución del proyecto.
2. Especificar la funcionalidad.
3. Aclarar ambigüedades y decisiones pendientes.
4. Preparar el plan de implementación.
5. Desglosar el plan en tareas.
6. Implementar siguiendo la spec.
7. Validar los criterios de aceptación y mantener la spec alineada con el resultado.

Para ver la estructura de carpetas, el proceso detallado y las plantillas de constitución, especificación, plan y tareas, consulta la [guía SDD](sdd/README.md).

## Usar las plantillas `AGENTS.md` y `MEMORY.md`

1. Copia `AGENTS-template.md` a la raíz del proyecto destino y renómbralo a `AGENTS.md`.
2. Completa la descripción, el stack, los comandos comprobados, las convenciones y las reglas propias del proyecto. Sustituye o elimina todos los marcadores y apartados que no apliquen.
3. Si quieres conservar contexto entre sesiones, copia `MEMORY-template.md` a la raíz como `MEMORY.md`. Rellénalo solo con información real del proyecto; mantenlo en unas 50 líneas como máximo y no guardes datos sensibles ni personales.
4. Mantén en `AGENTS.md` las reglas permanentes y en `MEMORY.md` el estado cambiante, las decisiones, los aprendizajes y los próximos pasos. Las instrucciones de `AGENTS.md` indican a OpenCode cuándo leer y actualizar la memoria.

La memoria es opcional: si todavía no hay información duradera que guardar, puedes crear `MEMORY.md` más adelante.

## Usar la plantilla del comando `feature`

1. Copia `commands/feature-template.md` al proyecto destino como `.opencode/commands/feature.md`.
2. Desde OpenCode, ejecuta `/feature` seguido de la descripción de la funcionalidad.
3. Revisa el plan que prepare el agente y apruébalo antes de pedir la implementación.

### Ejemplo de uso

En un proyecto de seguimiento de estudio, podrías solicitar:

```text
/feature mostrar el total de minutos estudiados esta semana
```

OpenCode preparará un plan para esa funcionalidad y esperará tu aprobación antes de modificar archivos.

La plantilla selecciona el agente `plan`, consulta los archivos `AGENTS.md` aplicables y lee `MEMORY.md` en la raíz del proyecto si existe. No modifica archivos antes de recibir aprobación.

### Consultar si se utilizará una skill

Al solicitar una funcionalidad, puedes preguntar también si hay una skill pertinente:

```text
/feature mostrar cuántos días he estudiado este mes. ¿Vas a utilizar alguna skill?
```

El agente debe indicar el ID de una skill disponible que sea relevante y por qué la usaría, o explicar que ninguna aplica. La plantilla genérica de esta carpeta no se activa por sí sola: primero hay que instalar una skill concreta en el proyecto.

## Usar la plantilla `SKILL.md`

1. Copia `skills/SKILL-template.md` al proyecto destino como `.opencode/skills/<skill-id>/SKILL.md`.
2. Usa para `<skill-id>` un identificador único en minúsculas y kebab-case, por ejemplo `local-dates`.
3. Completa `name` y `description`; la descripción debe explicar claramente cuándo es pertinente la skill. Sustituye los marcadores por instrucciones, pasos y comprobaciones específicas del proyecto.
4. Añade los archivos de referencia o scripts que necesite la skill junto a `SKILL.md`; las rutas escritas en ella son relativas a su directorio.

OpenCode puede anunciar una skill por su descripción y cargar sus instrucciones completas cuando resulte pertinente y esté permitido por la configuración del proyecto.

Para usar la [plantilla de skill SDD](skills/sdd/SKILL-template.md), cópiala al proyecto como `.agents/skills/sdd/SKILL.md`. Las plantillas de comandos SDD están enlazadas desde la [tabla de automatización](sdd/README.md); cópialas a `.opencode/commands/` y elimina el sufijo `-template`.

### Ejemplo de uso: `frontend-design`

Si la skill `frontend-design` está instalada y disponible en el proyecto, puedes invocarla directamente:

```text
@frontend-design mejora el diseño de la web, conserva las funcionalidades existentes y prioriza la accesibilidad y el diseño responsive
```

`@frontend-design` usa el ID de la skill; no funciona si la skill no está instalada o si su ID es distinto.

## Starter Pack de Skills

Estas skills de terceros son recomendaciones orientativas; no vienen incluidas ni se instalan automáticamente con estas plantillas. Revisa el repositorio, las instrucciones y los posibles scripts antes de instalar una skill, especialmente las que ayudan a buscar o instalar otras.

- [`find-skills` — Vercel Labs](https://github.com/vercel-labs/skills/tree/main/skills/find-skills): ayuda a descubrir skills del ecosistema y a encontrar cómo instalarlas.
- [`grill-me` — Matt Pocock](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me): entrevista al usuario para poner a prueba un plan o diseño y descubrir decisiones pendientes.
- [`frontend-design` — Anthropic](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/frontend-design/skills/frontend-design): ayuda a crear o rediseñar interfaces con una dirección visual intencionada.
- [`web-design-guidelines` — Vercel Labs](https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines): revisa interfaces frente a pautas de diseño web, UX y accesibilidad.
- [`systematic-debugging` — obra / Superpowers](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging): guía una depuración metódica, desde reproducir el problema hasta verificar la causa y la solución.

## Referencias sobre Skills

- [Agent Skills](https://agentskills.io/)
- [OpenCode — Skills](https://opencode.ai/v2/docs/skills)
- [skills.sh](http://skills.sh/)

Consulta la [documentación oficial de comandos](https://opencode.ai/docs/commands/) para más información sobre comandos y argumentos.

## MCP (Model Context Protocol)

MCP es un protocolo abierto para conectar agentes con servidores que proporcionan herramientas, recursos, prompts e instrucciones. Por ejemplo, un servidor puede dar acceso a documentación, un navegador, repositorios o servicios externos.

### Ventajas y seguridad

- **Estándar abierto e interoperable:** permite reutilizar integraciones entre clientes compatibles con MCP.
- **Acceso a datos y herramientas externas:** conecta el agente con capacidades que no tiene por sí solo.
- **Autenticación y permisos:** el protocolo y los clientes ofrecen mecanismos de control, pero no garantizan que cualquier servidor sea seguro. Revisa su origen y concede solo los permisos necesarios.
- **Ecosistema creciente:** hay servidores oficiales y de terceros para distintos servicios.

### Añadir servidores MCP

Puedes añadir servidores con la CLI —que actualiza la configuración del proyecto— o editando `opencode.json`/`opencode.jsonc` en la raíz. En OpenCode V2, los servidores se declaran bajo `mcp.servers`.

Ejemplos de alta con la CLI:

```sh
opencode mcp add context7 --url https://mcp.context7.com/mcp
opencode mcp add chrome-devtools -- npx -y chrome-devtools-mcp@latest --no-usage-statistics
opencode mcp list
```

La configuración se recarga automáticamente. Usa `/mcps` para consultar, conectar, desconectar o autenticar servidores; no suele ser necesario reiniciar OpenCode.

### Ejemplos de servidores

- **Chrome DevTools (Google):** permite al agente abrir y probar una web en el navegador, inspeccionar la consola y tomar capturas. [Repositorio oficial](https://github.com/ChromeDevTools/chrome-devtools-mcp).
- **Context7 (Upstash):** proporciona documentación actualizada de librerías y frameworks. Es especialmente útil cuando importan la versión o los cambios recientes de una API. No hace falta usarlo en todas las tareas: las llamadas MCP consumen contexto y para lógica propia del proyecto puede bastar con leer el código y la documentación local. [Documentación de Context7](https://context7.com/docs/resources/all-clients).
- **GitHub:** permite consultar y, según las herramientas y permisos concedidos, gestionar repositorios, issues y pull requests. [Documentación de GitHub](https://docs.github.com/en/copilot/how-tos/copilot-in-your-ide/copilot-for-common-tasks/use-the-github-mcp-server).
- **Figma:** proporciona contexto de diseños y componentes seleccionados para ayudar a implementarlos en código. [Documentación de Figma](https://developers.figma.com/docs/figma-mcp-server/).
- **Supabase:** conecta con proyectos y bases de datos de Supabase. Las acciones disponibles dependen del servidor y sus permisos; limita el acceso y ten especial cuidado con bases de datos de producción. [Documentación de Supabase](https://supabase.com/docs/guides/ai-tools/mcp).

### Ejemplo de configuración para el Diario de Estudio

Este ejemplo usa la estructura de OpenCode V2. La clave de Context7 se obtiene del entorno; no la escribas directamente en el archivo ni la subas al repositorio.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "chrome-devtools": {
        "type": "local",
        "command": [
          "npx",
          "-y",
          "chrome-devtools-mcp@latest",
          "--no-usage-statistics"
        ]
      },
      "context7": {
        "type": "remote",
        "url": "https://mcp.context7.com/mcp",
        "oauth": false,
        "headers": {
          "CONTEXT7_API_KEY": "{env:CONTEXT7_API_KEY}"
        }
      }
    }
  }
}
```

Si prefieres autenticar Context7 mediante OAuth, omite `oauth` y `headers` y sigue el flujo de autenticación que muestre OpenCode. Puedes revisar el estado con `opencode mcp list` o `/mcps`.

### Directorios de MCP

- [MCP Registry](https://registry.modelcontextprotocol.io/)
- [mcp.directory](https://mcp.directory/)
- [mcpservers.org](https://mcpservers.org/es)

Consulta la [documentación oficial de OpenCode sobre MCP](https://opencode.ai/v2/docs/mcp-servers) y la [documentación del protocolo MCP](https://modelcontextprotocol.io/).
