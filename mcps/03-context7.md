# Context7 MCP

[Context7](https://context7.com/docs/overview) permite consultar documentación actual de librerías y frameworks durante el desarrollo. Su MCP puede resolver el identificador de una librería y recuperar documentación relevante para una consulta.

## Instalar y conectar Context7

Desde la terminal, ejecuta el asistente oficial:

```bash
npx ctx7 setup --mcp --opencode --project
```

El comando configura Context7 como MCP para OpenCode en el proyecto actual. Para una configuración global, quita `--project`.

La instalación puede pedir autenticación. No guardes una clave de API directamente en un archivo versionado. Consulta la [guía oficial de instalación](https://context7.com/install) y la [referencia del CLI](https://context7.com/docs/clients/cli) para conocer las opciones actuales.

Después, inicia o reinicia OpenCode y ejecuta `/mcps` para comprobar la conexión.

![Configuración de Context7 MCP en OpenCode](../assets/17-opencode-mcp-context7-config.png)

## Consultar documentación

Puedes pedir explícitamente al agente que use Context7 añadiendo `use context7` al prompt. Por ejemplo, en modo `Build`:

```text
¿Cómo se deben proteger las rutas en Next.js? Usa Context7 para consultar la documentación actual.
```

OpenCode puede resolver la librería y consultar la documentación relacionada antes de responder.

## Ejemplo de respuesta

La consulta anterior produjo una respuesta sobre autenticación y autorización en Next.js. La respuesta completa, incluyendo las llamadas MCP y los fragmentos de código, está disponible en [Ejemplo de respuesta de Context7 sobre autenticación en Next.js](./ejemplos/respuesta-context7-nextjs-auth.md).

Ese archivo es un ejemplo de salida generada por OpenCode. No debe interpretarse como una guía universal de autenticación ni como una configuración obligatoria para todos los proyectos.

[← Anterior](02-playwright.md) | [Temario](../10-opencode-mcp.md) | [Siguiente →](04-instrucciones-del-agente.md)
