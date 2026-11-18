# Subagentes en OpenCode

Los subagentes son asistentes especializados que OpenCode ejecuta en sesiones hijas para investigar, revisar o resolver una parte concreta de una tarea. Trabajan con un contexto separado y devuelven un informe al agente principal.

Permiten:

- **Manejar un contexto limpio**: `Aislar contexto`, cada agente en su propia ventana, sin contaminar la conversación principal.
- **Especialización**:
  - `Especializar comportamiento`, un system prompt distinto para cada caso (ej. `security-auditor`, `exercise-creator`, `ui-test-writer`)
  - `Restringir herramientas`, se puede limitar que tools puede usar (solo `read/grep`, sin `write`, etc. )
- **Paralelismo**: `Paralelizar trabajo`, varios agentes corren a la vez sobre tareas independientes.
- **Reusabilidad**: `Reutilizar workflows`, se define una vez al experto en "X" y se lo invoca con `Agent(subagent_type: "X")`

Consulta la [documentación oficial de agentes](https://opencode.ai/v2/docs/agents) para conocer las opciones vigentes.

> **Contexto de los ejemplos:** los nombres, modelos, permisos y tareas mostrados son ejemplos. Adáptalos al proyecto y revisa los permisos antes de ejecutar un subagente.

## 1. Diferencia entre agente principal y subagente

OpenCode puede utilizar agentes con distintos modos:

| Modo       | Uso                                                                          |
| ---------- | ---------------------------------------------------------------------------- |
| `primary`  | Ejecuta la sesión principal y puede interactuar directamente con el usuario. |
| `subagent` | Solo se ejecuta en una sesión hija mediante la herramienta `subagent`.       |
| `all`      | Puede utilizarse como agente principal o como subagente.                     |

Los subagentes reciben un contexto independiente. El agente principal conserva el control de la tarea y recibe el informe del subagente cuando este termina o queda inactivo.

## 2. Crear un subagente mediante Markdown

Guarda un archivo Markdown dentro de `.opencode/agents/` en el proyecto. Por ejemplo:

```md
---
description: Revisa cambios sin modificar archivos.
mode: subagent
permissions:
  - action: edit
    resource: "*"
    effect: deny
  - action: shell
    resource: "*"
    effect: deny
---

Revisa los cambios actuales. Presenta los hallazgos por nivel de gravedad,
indicando el archivo y la línea correspondiente. No modifiques archivos.
```

El nombre del archivo se convierte en el identificador del agente. En este ejemplo, un archivo llamado `reviewer.md` crea el subagente `reviewer`.

Los agentes globales pueden guardarse en `~/.config/opencode/agents/`. Los agentes de proyecto deben guardarse en `.opencode/agents/` para compartirlos mediante Git.

## 3. Definir un subagente en `opencode.jsonc`

También puedes definirlo dentro de la propiedad `agents` de la configuración:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "agents": {
    "reviewer": {
      "description": "Revisa los cambios actuales.",
      "mode": "subagent",
      "system": "Identifica errores, regresiones y pruebas faltantes. No modifiques archivos.",
      "permissions": [
        {
          "action": "edit",
          "resource": "*",
          "effect": "deny",
        },
      ],
    },
  },
}
```

Integra `agents` en la configuración existente sin reemplazar otras propiedades del proyecto.

## 4. Invocar un subagente

El agente principal puede solicitar el uso de un subagente mediante una instrucción como:

```text
Usa el subagente reviewer para revisar los cambios actuales y reportar riesgos de regresión.
```

La disponibilidad del subagente depende de su configuración, visibilidad y permisos. Si no aparece en el catálogo, revisa el nombre, la ruta y el archivo de configuración.

## 5. Controlar qué subagentes puede ejecutar un agente

La acción `subagent` controla qué agentes hijos puede iniciar un agente principal. Por ejemplo, se puede denegar el acceso general y permitir únicamente un agente concreto:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "agents": {
    "orchestrator": {
      "mode": "primary",
      "permissions": [
        {
          "action": "subagent",
          "resource": "*",
          "effect": "deny",
        },
        {
          "action": "subagent",
          "resource": "reviewer",
          "effect": "allow",
        },
      ],
    },
  },
}
```

Las reglas se evalúan en orden y la última regla coincidente prevalece. Coloca primero las reglas generales y después las excepciones.

## 6. Permisos de un subagente

Los permisos del agente hijo controlan sus propias acciones. Puedes limitar las herramientas que necesita para su tarea:

- `read`, `glob` y `grep` para investigar archivos.
- `edit` para permitir o impedir modificaciones.
- `shell` para controlar comandos de terminal.
- `webfetch` y `websearch` para consultar información externa.
- `skill` para controlar la carga de Skills.
- `subagent` para impedir que cree otros subagentes.

Un subagente de revisión normalmente debe tener `edit: deny`. Un subagente de implementación puede necesitar permisos de edición, pero debe recibir instrucciones y límites claros.

## 7. Elegir el modelo y limitar pasos

Puedes asignar un modelo específico a un subagente:

```yaml
---
description: Revisa migraciones de base de datos.
mode: subagent
model: provider/model#variant
steps: 8
---
Revisa las migraciones y resume los riesgos de integridad y compatibilidad.
```

`steps` establece un máximo positivo de pasos del modelo. En el último paso OpenCode elimina las herramientas y solicita un resumen textual.

Si no se configura un modelo, el subagente puede heredar el modelo de la sesión principal.

Se puede crear desde `OpenCode` analizando en modo `Plan` y ejecutando en modo `Build`, ejemplo:

**Modo Plan**

```powershell
Crea un agente llamado react-best-practices.
Su trabajo es aplicar las mejores prácticas de React en todos los archivos que lo implementen dentro del proyecto.
Debe usar context7 para verificar si se están siguiendo buenas prácticas y las últimas recomendaciones en la documentacion.
```

Respuesta:

```powershell
Plan
Crearé el agente en la ubicación canónica de OpenCode V2:
.opencode/agents/react-best-practices.md
Configuración prevista:
- Nombre derivado: react-best-practices
- mode: all, para poder seleccionarlo como agente principal o invocarlo como subagente.
- Sin modelo fijo, para heredar el modelo de la sesión.
- Alcance: todos los archivos React/JSX/TSX del proyecto.
- Revisará y corregirá:
- Pureza de componentes y Hooks.
- Reglas de Hooks.
- Estado redundante o derivado.
- Effects innecesarios.
- Límites Server/Client de Next.js.
- Mutaciones, keys, composición y flujo de datos.
- Memoización prematura.
- Tipado, accesibilidad y responsabilidades de componentes.
- Antes de recomendar o modificar código:
1. Leerá la versión instalada de React.
2. Resolverá React mediante Context7.
3. Consultará la documentación oficial por cada tema relevante.
4. Consultará también la documentación local de Next.js cuando afecte Server Components o App Router.
- Respetará AGENTS.md, architecture.md y coding.md.
- Validará con ESLint, TypeScript y build cuando corresponda.
- Informará archivos modificados, verificaciones ejecutadas y documentación consultada.
Context7 confirma como recomendaciones actuales evitar estado derivado y Effects innecesarios, mantener el render puro y no introducir memo, useMemo o useCallback sin una necesidad demostrable.
Para crearlo, cambia al agente Build.
```

**Modo Build**:

```powershell
Ejecuta el plan
```

Respuesta

```powershell
Plan
Crearé el agente en la ubicación canónica de OpenCode V2:
.opencode/agents/react-best-practices.md
Configuración prevista:
- Nombre derivado: react-best-practices
- mode: all, para poder seleccionarlo como agente principal o invocarlo como subagente.
- Sin modelo fijo, para heredar el modelo de la sesión.
- Alcance: todos los archivos React/JSX/TSX del proyecto.
- Revisará y corregirá:
- Pureza de componentes y Hooks.
- Reglas de Hooks.
- Estado redundante o derivado.
- Effects innecesarios.
- Límites Server/Client de Next.js.
- Mutaciones, keys, composición y flujo de datos.
- Memoización prematura.
- Tipado, accesibilidad y responsabilidades de componentes.
- Antes de recomendar o modificar código:
1. Leerá la versión instalada de React.
2. Resolverá React mediante Context7.
3. Consultará la documentación oficial por cada tema relevante.
4. Consultará también la documentación local de Next.js cuando afecte Server Components o App Router.
- Respetará AGENTS.md, architecture.md y coding.md.
- Validará con ESLint, TypeScript y build cuando corresponda.
- Informará archivos modificados, verificaciones ejecutadas y documentación consultada.
Context7 confirma como recomendaciones actuales evitar estado derivado y Effects innecesarios, mantener el render puro y no introducir memo, useMemo o useCallback sin una necesidad demostrable.
Para crearlo, cambia al agente Build.
```

**react-best-practices.md**

```text
---
description: Applies current React best practices across the project's React files using Context7 verification.
mode: all
steps: 50
color: "#61dafb"
---

You are the `react-best-practices` agent for this repository. Review and improve React implementation quality across all applicable project files, especially `src/**/*.tsx`, `src/**/*.jsx`, `src/**/*.ts`, and `src/**/*.js` files that define or support React components, Hooks, or JSX.

## Required workflow

1. Read the repository `AGENTS.md`, `architecture.md`, and `coding.md` instructions that apply to the files being changed.
2. Inspect the installed React and Next.js versions from `package.json` before evaluating framework behavior.
3. Before making any recommendation or code change based on React behavior or best practices, resolve React with Context7 and query the official React documentation for the specific topic. Do not rely on memory when Context7 can verify the guidance.
4. When a change involves App Router, Server Components, Client Components, Suspense, routing, metadata, or other Next.js behavior, read the relevant local guide under `node_modules/next/dist/docs/` and use Context7 to verify current Next.js guidance when appropriate.
5. Inspect all relevant React files and their consumers before editing. Preserve existing behavior, public contracts, architecture boundaries, accessibility, and intentional project decisions.
6. Apply only justified improvements. Do not add abstractions, memoization, state, Effects, dependencies, or client boundaries without a concrete reason.
7. After editing, review the diff and run the narrowest relevant checks, expanding to the repository checks when the change affects application code or Server/Client boundaries.

## React review criteria

- Keep components and Hooks pure: no side effects during render, no mutation of props, state, Hook arguments, or values already passed to JSX.
- Follow the Rules of Hooks and preserve the unconditional top-level call order.
- Prefer derived values calculated during render over redundant state synchronized with an Effect.
- Use Effects only to synchronize with external systems or browser APIs; remove Effects that merely transform data or respond to events.
- Keep state as local as practical, model state transitions clearly, and avoid duplicating a source of truth.
- Use stable, data-based keys for lists; never use array indexes when items can be inserted, removed, or reordered.
- Keep Server Components as the default in this Next.js application. Add or retain `"use client"` only for state, event handlers, browser APIs, or client-only dependencies.
- Keep Client Components free of server-only modules, infrastructure, private configuration, persistence access, and non-serializable props.
- Prefer composition and `children` over unnecessary prop drilling or memoization.
- Do not introduce `memo`, `useMemo`, or `useCallback` automatically. Use them only when profiling, a Hook dependency, or a memoized child gives a concrete justification, and verify dependency arrays.
- Keep reusable components presentational and configurable through props. Follow the project's requirements for `className`, native element attributes, JSDoc, and public module barrels.
- Preserve accessible semantics, keyboard interaction, focus-visible states, labels, disabled/loading states, and correct event behavior.
- Avoid unstable values, accidental remounts, render-time mutations, stale closures, missing dependencies, and unnecessary re-renders.
- Keep types strict and avoid broad casts, implicit `any`, duplicated domain types, and leaking server data to the browser.

## Editing and verification rules

- Make focused fixes; do not rewrite unrelated files or alter product behavior merely for style preferences.
- If React documentation is ambiguous or a proposed optimization is not measurable, report the concern instead of making a speculative change.
- Check imports and dependency direction against the repository architecture before and after edits.
- Search for all consumers when changing component props, exported types, Hooks, or module boundaries.
- Run `npx eslint src` and `npx tsc --noEmit --incremental false` for code changes. Run `npm run build` when routing, configuration, rendering mode, or Server/Client boundaries are affected.
- Report the files changed, the React guidance verified through Context7, checks executed, and any remaining concerns.
```

Se debe reiniciar `OpenCode` y se ejecuta:

```powershell
Revisar tres componentes de React de forma aleatoria usando @react-best-practices. Usar tres agentes para realizar el trabajo mas rapido
```

## 8. Patrones recomendados

### Patrón Orquestador-Trabajador

El agente principal analiza una tarea, decide si manejarla directamente o delegarla, y usa la herramienta `Agent` para lanzar un subagente con un prompt específico.

### Revisión de código

Utiliza un subagente de solo lectura para detectar errores, regresiones, problemas de seguridad y pruebas faltantes.

### Investigación

Utiliza un subagente para explorar una parte del repositorio o consultar documentación sin mezclar ese análisis con la conversación principal.

### Validación especializada

Utiliza un subagente con herramientas concretas para revisar accesibilidad, UI, migraciones, APIs o criterios de aceptación.

La guía de [agentes personalizados](11-opencode-custom-agent.md) muestra un validador de criterios de aceptación que puede ejecutarse como agente especializado.

## 9. Recomendaciones de seguridad y mantenimiento

- Define una responsabilidad concreta para cada subagente.
- Usa `mode: subagent` cuando no deba utilizarse como agente principal.
- Restringe `edit` y `shell` si la tarea solo requiere lectura.
- Limita los agentes que puede invocar un orquestador.
- No incluyas claves, tokens ni secretos en el prompt o en la configuración.
- Revisa el informe del subagente antes de aplicar sus recomendaciones.
- Mantén los agentes de proyecto dentro del repositorio cuando deban compartirse con el equipo.

---

[← Anterior](15-opencode-formatters.md) | [Índice principal](README.md) | [Siguiente →](17-opencode-cron-jobs.md)
