# OpenCode: TUI (Terminal User Interface)

Esta guía se divide en documentos temáticos sobre la interfaz de terminal de OpenCode, sus sesiones, el contexto, las herramientas y el flujo `Plan`/`Build`.

> **Contexto de los ejemplos:** los comandos, atajos, nombres de archivos, respuestas y configuraciones son ejemplos didácticos. Pueden variar según la versión, el modelo, la configuración y el proyecto. Adáptalos antes de ejecutarlos.

Los atajos predeterminados pueden cambiar. Consulta la [documentación de la TUI](https://opencode.ai/v2/docs/cli/tui/) y la [referencia de atajos](https://opencode.ai/v2/docs/cli/keybinds/) si alguno no coincide.

## Temario

1. [Iniciar y editar el prompt](tui/01-iniciar-y-editar-el-prompt.md)
2. [Sesiones y panel lateral](tui/02-sesiones-y-panel-lateral.md)
3. [Contexto y compactación](tui/03-contexto-y-compactacion.md)
4. [Modo Shell](tui/04-modo-shell.md)
5. [Git, Undo y Redo](tui/05-git-undo-redo.md)
6. [`/init`, `AGENTS.md` e ignorados](tui/06-init-agents-e-ignorados.md)
7. [Plan y Build](tui/07-plan-y-build.md)
8. [Ejemplos de respuestas de Plan y Build](tui/ejemplos/respuestas-plan-build.md)
9. [Referencia de atajos](tui/08-referencia-de-atajos.md)

## Recorrido recomendado

1. Abre OpenCode desde la raíz del proyecto y documenta su objetivo antes de ejecutar `/init`.
2. Mantén las reglas globales en `AGENTS.md` y aporta archivos específicos con `@` cuando hagan falta.
3. Para tareas complejas, diseña y revisa el plan antes de pasar a `Build`; luego valida los cambios con las herramientas del proyecto.
4. Configura Git antes de depender de `/undo` y `/redo` para restaurar archivos.
5. Antes de compactar una sesión, conserva las decisiones y restricciones importantes.

Para tareas con muchas decisiones, puede convenir un modelo de mayor capacidad durante `Plan` y uno más rápido durante `Build`, una vez que los pasos estén claros.

---

[← Anterior](05_opencode-context.md) | [Temario](README.md) | [Siguiente →](07_opencode-assisted-development.md)
