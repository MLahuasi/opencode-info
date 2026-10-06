# Preparar el laboratorio OpenDayCare

Este documento reúne la preparación específica del proyecto utilizado como ejemplo para las guías de MCP.

## Crear e iniciar la aplicación

Crear una aplicación Next.js:

```bash
npx create-next-app@latest open-daycare
# Dar clic en opciones recomendadas
```

Una vez creada, inicia la aplicación:

```bash
npm run dev
```

En el [repositorio del proyecto](https://github.com/MLahuasi/opencode-daycare), Next.js mantiene un bloque de instrucciones para agentes dentro de [`AGENTS.md`](https://github.com/MLahuasi/opencode-daycare/blob/main/AGENTS.md). Ese bloque apunta a la documentación local de la versión instalada, ubicada en `node_modules/next/dist/docs/`, y puede actualizarse al ejecutar `next dev`.

![Documentación local de Next.js para agentes](../assets/12-open-care-next-ia-docs.png)

El archivo [`CLAUDE.md`](https://github.com/MLahuasi/opencode-daycare/blob/main/CLAUDE.md) referencia `AGENTS.md` para que Claude Code también considere las instrucciones del proyecto.

Las plantillas HTML y las imágenes de referencia se guardan en [`references/`](https://github.com/MLahuasi/opencode-daycare/tree/main/references). Se pueden preparar con herramientas de diseño asistido por IA, como [Claude Design](https://claude.ai/design), [V0](https://v0.app/), [Lovable](https://lovable.dev/), [Google Stitch](https://stitch.withgoogle.com/) o [Bolt](https://bolt.new/).

## Documentación relacionada

- [Configuración de Playwright MCP](../mcps/02-playwright.md)
- [Ejemplo de reglas `AGENTS.md`](../mcps/ejemplos/agents-mcp-opencode-daycare.md)
- [Arquitectura del laboratorio](./organice-architecture.md)

[Temario MCP](../10-opencode-mcp.md) | [Siguiente →](README.md)
