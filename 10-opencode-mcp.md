# MCPs e instrucciones para agentes en OpenCode

Esta guía introduce el Model Context Protocol (MCP), su configuración en OpenCode y la forma de combinar herramientas MCP con Skills e instrucciones de agentes.

Los ejemplos se basan en el laboratorio [OpenDayCare](./opencode-daycare/README.md). Las respuestas generadas por OpenCode y las configuraciones específicas del laboratorio se identifican como ejemplos para no confundirlas con reglas generales.

## Temario

1. [Conceptos y configuración de MCP](./mcps/01-conceptos-y-configuracion.md)
2. [Playwright MCP](./mcps/02-playwright.md)
3. [Context7 MCP](./mcps/03-context7.md)
4. [MCP e instrucciones del agente](./mcps/04-instrucciones-del-agente.md)
5. [Ejemplos de respuestas y configuración](#ejemplos-de-respuestas-y-configuracion)
6. [Laboratorio OpenDayCare](./opencode-daycare/README.md)

## Orden recomendado

1. Lee los conceptos y la configuración común de MCP.
2. Configura Playwright si necesitas revisar una aplicación en el navegador.
3. Configura Context7 si necesitas consultar documentación actualizada.
4. Combina esas herramientas con Skills e instrucciones de `AGENTS.md`.
5. Revisa los ejemplos para distinguir configuración general de respuestas generadas y reglas específicas del laboratorio.

## Ejemplos de respuestas y configuración

- [Respuesta de Context7 sobre autenticación en Next.js](./mcps/ejemplos/respuesta-context7-nextjs-auth.md) — transcripción resumida de una consulta y su respuesta.
- [Ejemplo de `AGENTS.md` para OpenDayCare](./mcps/ejemplos/agents-mcp-opencode-daycare.md) — configuración específica del laboratorio.
- [Preparación del laboratorio](./opencode-daycare/setup.md) — creación de la aplicación, documentación local y referencias iniciales.

## Referencias generales

- [Documentación de servidores MCP de OpenCode](https://opencode.ai/docs/mcp-servers/)
- [Documentación de configuración de OpenCode](https://opencode.ai/docs/config/)
- [Documentación de Skills de OpenCode](https://opencode.ai/docs/skills/)
- [Documentación de Playwright MCP](https://playwright.dev/docs/getting-started-mcp)
- [Documentación de Context7](https://context7.com/docs/overview)
