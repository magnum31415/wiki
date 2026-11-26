# AGENTS.md — [Nombre del proyecto]

[En una o dos frases: qué es el proyecto, para quién es y cuál es su objetivo.]

## Stack, entorno y estructura

- **Stack y versiones:** [tecnologías y versiones importantes].
- **Entorno:** [runtime, plataforma u otros requisitos].
- **Estructura:** [carpetas y archivos importantes cuyo propósito no sea obvio].

## Comandos

Ejecutar desde: [directorio, normalmente la raíz del proyecto].

- **Requisitos previos:** [instalación o configuración necesaria].
- **Ejecutar:** `[comando exacto]`
- **Probar:** `[comando exacto]`
- **Lint:** `[comando exacto]`
- **Compilar:** `[comando exacto]`

Indica solo comandos comprobados. Si alguna tarea no tiene un comando configurado, déjalo claro.

## Convenciones

- **Idioma:** [comentarios, documentación e interfaz].
- **Estilo y nombres:** [reglas concretas].
- **Patrones:** [patrones a seguir y archivos de referencia].

## Reglas de dominio y trampas conocidas

- [Restricciones o hechos importantes que no sean evidentes leyendo el código.]
- [Errores frecuentes que hay que evitar.]

## Reglas de SDD (si se utiliza en este proyecto)

- Antes de modificar código, lee `docs/constitution.md` si existe y la especificación activa de la tarea (`specs/NNN-<nombre>/spec.md`); revisa también `plan.md` y `tasks.md` si están disponibles.
- Identifica la carpeta de spec que corresponde a la tarea; no elijas una spec solo por coincidir con el patrón `specs/NNN-*/`.
- Si no existe una spec para una funcionalidad nueva, o la spec es ambigua o contradice la solicitud, aclárala y actualízala antes de implementar.
- Mantén la implementación, las tareas y los criterios de aceptación alineados con la spec.

## Forma de trabajar

- Revisa el estado del repositorio antes de editar y conserva los cambios preexistentes.
- Planifica antes de [cambios complejos, amplios o difíciles de revertir].
- Mantén los cambios pequeños y centrados en la solicitud.
- Al terminar, resume qué cambió, qué verificaste y qué no pudiste verificar.

## Memoria

- Al empezar, lee el `MEMORY.md` de la raíz del proyecto si existe.
- Al terminar cada tarea, actualiza el `MEMORY.md` con cualquier cambio relevante de estado, decisión, aprendizaje o próximos pasos. Si no hay nada nuevo que registrar, déjalo sin cambios; no inventes información.
- Si no existe, créalo solo cuando haya información duradera que merezca conservarse.
- Mantenlo breve (máximo ~50 líneas): resume o elimina lo que ya no aporte.
- Si algo se convierte en una regla permanente, propón moverlo a `AGENTS.md`.
- No guardes datos sensibles, credenciales, tokens ni datos personales.

## Git y seguridad

- **Ramas:** [política de ramas del proyecto].
- No hagas commit ni `push` sin autorización explícita.
- No incluyas credenciales, tokens ni otros secretos en archivos o respuestas.

## Límites

- ✅ **Siempre:** actualizar `MEMORY.md` al terminar cada tarea; [otras acciones que el agente puede realizar sin preguntar].
- ⚠️ **Pregunta antes de:** añadir dependencias, hacer cambios destructivos o migraciones de datos, cambiar formatos públicos, desplegar o ampliar el alcance solicitado.
- 🚫 **Nunca:** compartas ni subas contraseñas, API keys, tokens, credenciales u otros secretos a Git, GitHub, GitLab ni a otros servicios; [añade otras acciones prohibidas propias del proyecto].

## Verificación

- **Comprobaciones requeridas:** [comandos exactos y resultado esperado].
- Si no puedes ejecutar una comprobación, explica el motivo y no afirmes que pasó.
