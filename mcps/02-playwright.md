# Playwright MCP

[Playwright MCP](https://playwright.dev/docs/getting-started-mcp) permite que OpenCode abra un navegador e interactúe con una aplicación web.

## Configurar Playwright en OpenCode

Desde la raíz del proyecto, agrega esta configuración local a `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": ["npx", "-y", "@playwright/mcp@latest"],
      "enabled": true
    }
  }
}
```

Esta es la estructura de OpenCode. La documentación de Playwright puede mostrar configuraciones genéricas para otros clientes MCP; en OpenCode la entrada se declara dentro de `mcp`.

En el laboratorio `opencode-daycare`, la configuración se encuentra en [`opencode.json`](https://github.com/MLahuasi/opencode-daycare/blob/main/opencode.json). Para la configuración global, consulta la [guía de configuración de OpenCode](https://opencode.ai/docs/config/).

## Resolver problemas de `PATH` con NVM

Si OpenCode no encuentra `npx` porque Node.js se administra con NVM, indica la ruta completa al ejecutable. Sustituye las rutas y versiones por las de tu equipo.

### Windows con NVM

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": [
        // <NVM_HOME>\<versión-node>\npx.cmd
        "C:\\Users\\Developer\\AppData\\Roaming\\nvm\\v22.23.2\\npx.cmd",
        "-y",
        "@playwright/mcp@latest"
      ],
      "enabled": true,
      "environment": {
        // <NVM_HOME>\<versión-node>;<PATH-del-sistema>
        "PATH": "C:\\Users\\Developer\\AppData\\Roaming\\nvm\\v22.23.2;C:\\Windows\\System32;C:\\Windows"
      }
    }
  }
}
```

### Linux o macOS con NVM

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": [
        // <NVM_HOME>/versions/node/<versión-node>/bin/npx
        "/Users/strider/.nvm/versions/node/v22.20.0/bin/npx",
        "-y",
        "@playwright/mcp@latest"
      ],
      "enabled": true,
      "environment": {
        // <NVM_HOME>/versions/node/<versión-node>/bin:<PATH-del-sistema>
        "PATH": "/Users/strider/.nvm/versions/node/v22.20.0/bin:/usr/local/bin:/usr/bin:/bin"
      }
    }
  }
}
```

Después de guardar la configuración, reinicia OpenCode y usa `/mcps` para confirmar que Playwright esté conectado.

![Configuración de Playwright MCP en OpenCode](../assets/13-opencode-mcp-playwright-config.png)

## Revisar una aplicación

Con la aplicación en ejecución, por ejemplo mediante `npm run dev`, en modo `Build` puedes pedir:

```text
Utiliza el MCP Playwright y revisa el home (/)
```

OpenCode puede iniciar la aplicación, abrir el navegador y revisar la página solicitada. Para guardar capturas, especifica el formato o directorio.

En el laboratorio, las capturas de trabajo se guardan en `.playwright-mcp/`. Si son temporales, agrega ese directorio a `.gitignore`; conserva en Git únicamente las imágenes que quieras incluir como documentación.

También puedes solicitar:

```text
captura un screenshot
```

Ejemplos de resultados visuales del laboratorio:

![Home de escritorio](../assets/14-opencode-mcp-local-screenshot-home-desktop.png)

![Home móvil](../assets/15-opencode-mcp-local-screenshot-home-mobile.png)

![Captura adicional del Home](../assets/16-opencode-mcp-local-screenshot-home-screenshot.png)

Para que el agente guarde las capturas en un directorio específico, agrega una regla en [`AGENTS.md`](https://github.com/MLahuasi/opencode-daycare/blob/main/AGENTS.md):

```md
## MCPs

- Guarda las capturas y los archivos de Playwright en `.playwright-mcp/`.
```

[← Anterior](01-conceptos-y-configuracion.md) | [Temario](../10-opencode-mcp.md) | [Siguiente →](03-context7.md)
