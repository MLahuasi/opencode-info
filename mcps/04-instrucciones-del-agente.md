# MCP e instrucciones del agente

## Relación entre MCP, Skills y agentes

Las Skills `spec` y `spec-impl` se explican en [Spec Driven Development](../09-opencode-spec-driven-development.md). Este documento se enfoca en combinar esas instrucciones con herramientas MCP.

- MCP proporciona herramientas externas, como un navegador o documentación actualizada.
- Una Skill define un procedimiento reutilizable.
- Un agente interpreta las instrucciones y utiliza las herramientas disponibles.

En [`AGENTS.md`](https://github.com/MLahuasi/opencode-daycare/blob/main/AGENTS.md) se puede indicar cuándo debe usar cada MCP.

## Ejemplo de reglas del proyecto

El archivo [Ejemplo de reglas MCP para OpenDayCare](./ejemplos/agents-mcp-opencode-daycare.md) conserva una configuración de ejemplo con reglas para Skills, MCP, arquitectura, React, datos y estilos.

Esas reglas pertenecen al laboratorio `opencode-daycare`. Deben adaptarse a la estructura real de cada repositorio y no representan requisitos generales de MCP.

## Inicializar OpenCode

Desde la raíz del proyecto, abre OpenCode. Si solicitas `/init`, indica que debe revisar y completar las instrucciones existentes sin eliminar reglas del proyecto ni reemplazar el contenido de `AGENTS.md`.

Puedes pedir primero el análisis en modo `Plan`:

```text
Analiza el proyecto antes de ejecutar /init. @AGENTS.md ya contiene instrucciones que deben preservarse. Respeta su contenido, estructura y referencias a documentación. Completa únicamente la información necesaria para inicializar el proyecto, sin sobrescribir, duplicar ni eliminar reglas existentes.
```

Después ejecuta `/init` y revisa el archivo antes de guardar los cambios. Si el cambio de configuración quedó correcto, regístralo en Git:

```bash
git add .
git commit -m "OpenCode MCP and project instructions"
```

[← Anterior](03-context7.md) | [Temario](../10-opencode-mcp.md)
