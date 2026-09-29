# Agentes personalizados en OpenCode

Un agente personalizado es un asistente especializado para una tarea y un flujo de trabajo. Puede tener instrucciones, permisos y un modo de uso propios. Esta guía crea un validador de criterios de aceptación y muestra cómo ejecutarlo desde OpenCode.

Consulta la [documentación oficial de agentes](https://opencode.ai/docs/agents/) para ver todas las opciones vigentes.

## Temario

1. [Definir un agente Markdown](#1-definir-un-agente-markdown)
2. [Crear un agente](#2-crear-un-agente)
3. [Crear el validador de criterios de aceptación](#3-crear-el-validador-de-criterios-de-aceptación)
4. [Validar una Spec](#4-validar-una-spec)

---

## 1. Definir un agente Markdown

Los agentes Markdown se configuran con metadatos YAML al inicio del archivo y las instrucciones en el cuerpo. El nombre del archivo se convierte en el nombre del agente.

```md
---
description: Revisa cambios de código e identifica riesgos sin modificarlos.
mode: subagent
permission:
  edit: deny
  bash: deny
---

Revisa los cambios de código y presenta los hallazgos por nivel de gravedad, con referencias a los archivos y líneas correspondientes.

Considera errores, casos límite, rendimiento y seguridad. No modifiques archivos ni ejecutes comandos de shell.
```

`mode: subagent` permite que un agente principal lo invoque como subagente. Para trabajar con el agente en una sesión principal también, se puede configurar `mode: all`. Los subagentes se pueden invocar manualmente con `@nombre-del-agente` o automáticamente cuando el agente principal lo considere pertinente.

Las opciones `edit: deny` y `bash: deny` restringen las modificaciones y los comandos de shell. Adapta los permisos a la tarea que ejecutará el agente.

## 2. Crear un agente

### 2.1 Desde la terminal

Desde la raíz del proyecto, ejecuta:

```bash
opencode agent create
```

El asistente pregunta dónde guardarlo, qué tarea realizará y qué permisos necesita. Selecciona el alcance del proyecto para compartirlo mediante Git.

### 2.2 Desde OpenCode

En modo `Build`, solicita la creación del agente con su responsabilidad, permisos y alcance. Por ejemplo:

```text
Crear un agente de proyecto para validar los criterios de aceptación de una spec.

- Revisar cada elemento de `Acceptance criteria`.
- Corregir la implementación cuando sea necesario.
- Marcar solo los criterios que hayan sido verificados y dejar sin marcar los que no se puedan comprobar.
- Usar Context7 para validar recomendaciones actuales de Next.js.
- Usar Playwright MCP para validar UI, interacción y comportamiento visual.
- Usar un modelo con visión para comparar screenshots cuando la fidelidad visual forme parte de la spec.
- Configurar el agente únicamente a nivel de proyecto, no global.
```

---

## 3. Crear el validador de criterios de aceptación

El agente de este ejemplo se llama `spec-acceptance-validator`. Su descripción indica cuándo usarlo; las instrucciones detallan cómo inspeccionar la Spec, reunir evidencia, corregir incumplimientos y actualizar los criterios.

El archivo del agente es [spec-acceptance-validator.md](https://github.com/MLahuasi/opencode-daycare/blob/main/.opencode/agent/spec-acceptance-validator.md). En este repositorio está guardado en `.opencode/agent/`, una variante singular heredada que OpenCode todavía reconoce ([guía de migración V2](https://opencode.ai/v2/docs/migrate-v1/)). Para agentes nuevos, la ruta recomendada actualmente es `.opencode/agents/`.

El agente se puede invocar directamente desde OpenCode:

```text
@spec-acceptance-validator 01-home-feed.md
```

![](./assets/19-opencode-agent-custom.png)

### 3.1 Crear un comando para el agente

Un comando personalizado ofrece un atajo para ejecutar el agente con una instrucción predefinida. El comando de este ejemplo está en [spec-acceptance-validator.md](https://github.com/MLahuasi/opencode-daycare/blob/main/.opencode/command/spec-acceptance-validator.md) y lo invoca así:

```text
/spec-acceptance-validator 01-home-feed.md
```

El repositorio guarda el comando en `.opencode/command/`, carpeta singular heredada que OpenCode todavía reconoce ([documentación de comandos](https://opencode.ai/v2/docs/commands)). Para comandos nuevos, OpenCode recomienda `.opencode/commands/`.

![](./assets/20-opencode-command-custom.png)

Después de guardar los archivos, comprueba que el agente aparezca al escribir `@` y que el comando aparezca al escribir `/`. Si no aparecen, revisa la ubicación y el nombre de los archivos; reinicia OpenCode si hace falta.

## 4. Validar una Spec

El agente debe revisar individualmente los criterios de aceptación de [01-home-feed.md](https://github.com/MLahuasi/opencode-daycare/blob/main/specs/01-home-feed.md). La lista siguiente muestra su estado inicial:

```md
## Acceptance criteria

- [ ] `/` carga sin errores de servidor, consola o hidratación.
- [ ] La página muestra sidebar de 248px y feed con ancho máximo de 760px en escritorio.
- [ ] La página muestra barra de navegación inferior en una pantalla móvil.
- [ ] El contenido visible coincide con los datos mock de la plantilla: Caro Giménez, Sala Soles, 12 niños, martes 17 jun y tres publicaciones.
- [ ] Los corazones y contadores usan el tratamiento coral definido en el HTML de referencia.
- [ ] Las fuentes Fredoka y Nunito se cargan mediante `next/font/google`.
- [ ] Los botones y enlaces visuales tienen estados `hover`, `active` y `focus-visible` sin handlers ficticios.
- [ ] El feed contiene landmarks semánticos, un único H1, `aria-current` para Feed y nombre accesible para el control de cierre de sesión.
- [ ] No se implementan autenticación, base de datos, persistencia ni rutas adicionales.
- [ ] `npx eslint app` termina correctamente.
- [ ] `npx tsc --noEmit --incremental false` termina correctamente.
- [ ] `npm run build` termina correctamente.
- [ ] Las capturas de verificación se almacenan bajo `.playwright-mcp/Home/`.
```

Para iniciar la revisión en modo `Build`, ejecuta el comando:

```text
/spec-acceptance-validator 01-home-feed.md
```

Cuando termina la validación, el agente actualiza cada casilla solo si comprobó su criterio. Este es el estado resultante del ejemplo:

```md
## Acceptance criteria

- [x] `/` carga sin errores de servidor, consola o hidratación.
- [x] La página muestra sidebar de 248px y feed con ancho máximo de 760px en escritorio.
- [x] La página muestra barra de navegación inferior en una pantalla móvil.
- [x] El contenido visible coincide con los datos mock de la plantilla: Caro Giménez, Sala Soles, 12 niños, martes 17 jun y tres publicaciones.
- [x] Los corazones y contadores usan el tratamiento coral definido en el HTML de referencia.
- [x] Las fuentes Fredoka y Nunito se cargan mediante `next/font/google`.
- [x] Los botones y enlaces visuales tienen estados `hover`, `active` y `focus-visible` sin handlers ficticios.
- [x] El feed contiene landmarks semánticos, un único H1, `aria-current` para Feed y nombre accesible para el control de cierre de sesión.
- [x] No se implementan autenticación, base de datos, persistencia ni rutas adicionales.
- [x] `npx eslint app` termina correctamente.
- [x] `npx tsc --noEmit --incremental false` termina correctamente.
- [x] `npm run build` termina correctamente.
- [x] Las capturas de verificación se almacenan bajo `.playwright-mcp/Home/`.
```
