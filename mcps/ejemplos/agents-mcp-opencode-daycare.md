# Ejemplo de `AGENTS.md` para OpenDayCare

> **Ejemplo de configuración de un laboratorio**
>
> Estas reglas fueron utilizadas en `opencode-daycare`. No son requisitos de MCP ni deben copiarse sin adaptar rutas, agentes y convenciones al proyecto correspondiente.

```md
## Workflow (MCPs)

- Para funcionalidades grandes, cargar `spec`; para implementar una spec aprobada, cargar `spec-impl` y respetar sus pausas de revisión.
- Consulta Context7 para APIs que dependan de la versión y usa Playwright cuando la funcionalidad requiera interacción o revisión visual en el navegador.
- Validador de aceptación del proyecto: agente `@spec-acceptance-validator` (`.opencode/agent/spec-acceptance-validator.md`) y comando `/spec-acceptance-validator <spec>` (`.opencode/command/spec-acceptance-validator.md`). Corrige incumplimientos y marca solo los criterios que haya verificado.
- Los nombres de specs se relacionan con el módulo, no con una captura o prototipo.

## Arquitectura

- Usar feature-first: `app/components/ui` para UI genérica, `app/shared` para código transversal y `app/features/<domain>` para cada dominio.
- Los mocks estáticos se guardan en `app/data/mocks`; la persistencia JSON editable, en `app/infrastructure/persistence/json/data`.
- Una feature puede contener `components`, `types`, `schemas`, `actions`, `services` y `utils`.
- Las features no dependen de internals de otras features. Exponer su API pública mediante `index.ts` y mover lo común a `app/shared`.
- Los archivos especiales de App Router mantienen su convención de Next.js; la organización interna no crea rutas sin `page` o `route`.

## Código

- Nombres de código y archivos en inglés. Mantener responsabilidades claras, bajo acoplamiento y evitar abstracciones o cambios no solicitados.
- Separar presentación, datos, validación, estado y lógica de negocio cuando mezclarlo reduzca claridad.

### Documentación

- APIs exportadas y componentes reutilizables DEBEN tener JSDoc.
- El JSDoc de funciones incluye descripción, `@param`, cada prop propia recibida y `@returns`.
- Si las props extienden atributos nativos, indicarlo. Usar `//` solo para decisiones o lógica no evidente.

### React

- Componentes reutilizables DEBEN aceptar y combinar `className?: string`.
- Eventos configurables se reciben por props; no agregar handlers, enlaces ni navegación ficticios.
- Mantener Server Components por defecto. Usar `"use client"` solo para estado, eventos, hooks o APIs del navegador.
- Los elementos interactivos incluyen los estados visuales aplicables: `hover`, `focus-visible`, `disabled`, `loading` o `cursor-pointer`.

### Datos y configuración

- Componentes NO DEBEN contener datos mock o de negocio. Ubicarlos tipados en `app/data/mocks` o recibirlos por props.
- Datos mock incluyen nombres, fechas, cantidades, publicaciones, etiquetas variables y opciones de navegación.
- Configuración compartida usa constantes; configuración de entorno usa variables de entorno.

### Estilos

- Usar Tailwind para la mayoría de estilos y layout, conforme a la guía local de Next.js.
- Usar CSS Modules colocados junto al componente cuando estilos o variantes complejos no sean claros con utilities.
- `app/globals.css` se reserva para Tailwind, reset, fuentes y tokens globales.
- Colores, sombras y gradientes compartidos usan tokens semánticos. No usar estilos inline salvo valores calculados dinámicamente.
```

[← Volver a las instrucciones del agente](../04-instrucciones-del-agente.md) | [Temario](../../10-opencode-mcp.md)
