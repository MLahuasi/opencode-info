# `/init`, `AGENTS.md` e ignorados

> **Ejemplo:** los nombres y contenidos mostrados aquí ilustran una posible configuración. `/init` puede crear o actualizar archivos según el proyecto y no sustituye la revisión manual.

## 1. Ejecutar `/init`

El comando analiza el proyecto y puede crear o actualizar `AGENTS.md`:

```text
/init
```

`AGENTS.md` contiene instrucciones y contexto que OpenCode puede utilizar para trabajar con el proyecto. Revisa el archivo antes de aceptarlo como documentación válida.

## 2. Aportar contexto inicial con `README.md`

Crear o revisar un `README.md` antes de ejecutar `/init` es una buena práctica cuando el proyecto necesita explicar su propósito, pero no es un requisito técnico universal.

El README puede describir:

- El propósito del proyecto.
- Qué se quiere construir.
- Las tecnologías utilizadas.
- La estructura general.
- Las características relevantes del repositorio.

Ejemplo didáctico:

```md
# Mi primera página web

Página de ejemplo para explorar OpenCode.

## Tecnologías usadas

- HTML
- CSS
- JavaScript
```

Después de revisar el README, ejecuta desde OpenCode:

```text
/init
```

## 3. Ejemplo de respuesta de `/init`

La siguiente salida es un ejemplo de respuesta de OpenCode para un proyecto HTML estático. No es una plantilla normativa de `AGENTS.md`:

```md
# AGENTS.md

- This is a minimal static HTML demo; `index.html` is the only application entrypoint.
- There is no package manager, build step, dev server, lint, typecheck, or test suite. Verify changes by opening `index.html` in a browser or serving the repository with any local static file server.
- `index.html` currently uses inline CSS only. Although `README.md` lists Tailwind CSS and JavaScript, neither is wired into the page.
- Repository prose is written in Spanish; preserve that language when updating `README.md`.
```

## 4. Mantener `AGENTS.md` pequeño

El contenido de `AGENTS.md` forma parte del contexto que OpenCode proporciona al modelo. Es preferible mantener únicamente reglas globales realmente importantes, como:

- Arquitectura principal.
- Convenciones generales.
- Restricciones importantes.
- Tecnologías utilizadas.
- Reglas que deben aplicarse siempre.

## 5. Separar reglas específicas

Cuando existe documentación extensa, mantenla en archivos separados:

```text
naming_rules.md
architecture_rules.md
```

Cuando sea necesario utilizar una regla, puedes referenciarla explícitamente:

```text
@naming_rules.md
```

o:

```text
@architecture_rules.md
```

Así el agente puede cargar información específica cuando sea necesaria sin mantenerla permanentemente en `AGENTS.md`.

## 6. Patrones a ignorar

OpenCode respeta las declaraciones existentes en:

```text
.gitignore
```

Si un archivo debe formar parte del repositorio Git pero no debe considerarse durante las búsquedas de OpenCode, puede utilizarse:

```text
.ignore
```

### 6.1. Ejemplo de `.ignore`

Supongamos un monorepo con estas carpetas:

```text
auth/
frontend/
payments/
shared/
```

Si `auth` no es necesario para la tarea actual, el patrón siguiente puede colocarse en `.ignore`:

```text
auth/**
```

El directorio continúa formando parte del repositorio Git porque no se ha excluido en `.gitignore`, pero OpenCode puede omitirlo en sus búsquedas.

Esto ayuda a evitar búsquedas innecesarias, archivos irrelevantes, ruido y contexto adicional.

### 6.2. Diferencia práctica

`.gitignore` indica a Git qué archivos no rastreados debe ignorar; no deja de rastrear archivos que ya fueron agregados al repositorio.

`.ignore` permite afinar las búsquedas de OpenCode. Sus patrones y su interacción con `.gitignore` dependen de la implementación y la versión, por lo que conviene verificarlos antes de utilizarlos como una regla crítica.

Este mecanismo resulta especialmente útil en monorepos, repositorios grandes, proyectos con varios servicios, directorios generados y componentes que no forman parte de la tarea actual.

---

[← Anterior](05-git-undo-redo.md) | [Temario](../06_opencode_tui.md) | [Siguiente →](07-plan-y-build.md)
