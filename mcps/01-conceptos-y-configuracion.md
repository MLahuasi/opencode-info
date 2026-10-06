# Conceptos y configuración de MCP

## Objetivo

El Model Context Protocol (MCP) permite conectar OpenCode con herramientas externas. Esta guía explica la configuración común antes de instalar un servidor específico.

## Configurar servidores MCP

OpenCode admite servidores locales y remotos. Ambos se configuran bajo la propiedad `mcp` de `opencode.json`.

Consulta la [documentación de servidores MCP de OpenCode](https://opencode.ai/docs/mcp-servers/) para conocer las opciones disponibles.

Desde la raíz del proyecto, una configuración mínima puede tener esta forma:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "server-name": {
      "type": "local",
      "command": ["comando", "--opcion"],
      "enabled": true
    }
  }
}
```

La configuración concreta depende del servidor. Revisa siempre su documentación oficial y evita guardar claves directamente en archivos versionados.

## Comprobar la conexión

Después de guardar la configuración, inicia o reinicia OpenCode y ejecuta:

```text
/mcps
```

El comando permite revisar los servidores configurados y las herramientas que exponen.

Para conocer la configuración global y las opciones generales, consulta la [guía de configuración de OpenCode](https://opencode.ai/docs/config/).

## Guías relacionadas

- [Configurar Playwright MCP](./02-playwright.md)
- [Configurar Context7 MCP](./03-context7.md)
- [Usar MCP en las instrucciones del agente](./04-instrucciones-del-agente.md)

[Temario](../10-opencode-mcp.md) | [Siguiente →](02-playwright.md)
