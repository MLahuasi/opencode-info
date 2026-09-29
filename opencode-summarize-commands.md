# Resumen de comandos e instrucciones

Este documento reúne comandos e instrucciones que aparecen en la documentación del proyecto. Las filas consolidan ejemplos repetidos con distintos nombres de ramas o funcionalidades; cada una enlaza a un solo Markdown de referencia.

> Los valores entre `< >` son marcadores que se deben sustituir. Los comandos personalizados de OpenCode, como `/spec` o `/worktree`, solo están disponibles si se configuraron para el proyecto. Las rutas de la aplicación, como `/kids` o `/home`, no se consideran comandos de OpenCode.

## Git

| Comando | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `git init` | Inicia un repositorio en la carpeta actual. | `git init` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `git status` | Muestra los cambios pendientes y el estado de la rama. | `git status` | [08-opencode-github.md](./08-opencode-github.md) |
| `git diff` | Revisa cambios en archivos rastreados; `--check` busca errores de espacios. | `git diff --check` | [opencode-daycare/email.md](./opencode-daycare/email.md) |
| `git add <ruta>` | Prepara archivos para el próximo commit. | `git add .` | [opencode-daycare/login-activate.md](./opencode-daycare/login-activate.md) |
| `git commit -m "<mensaje>"` | Guarda los cambios preparados con un mensaje descriptivo. | `git commit -m "Implement feature"` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git switch -c <rama>` | Crea una rama y cambia a ella. La documentación también muestra `git checkout -b <rama>`. | `git switch -c feature/colors` | [08-opencode-github.md](./08-opencode-github.md) |
| `git switch <rama>` | Cambia a una rama existente. En ejemplos anteriores también aparece `git checkout <rama>`. | `git switch main` | [opencode-daycare/home.md](./opencode-daycare/home.md) |
| `git branch` / `git branch -r` | Lista ramas locales o remotas. | `git branch -r` | [08-opencode-github.md](./08-opencode-github.md) |
| `git branch -d <rama>` | Elimina una rama local ya integrada. | `git branch -d feature/colors` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git merge <rama>` | Integra los cambios de otra rama en la rama actual. | `git merge feature/colors` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git worktree add <ruta>` | Crea un directorio de trabajo adicional asociado al repositorio. | `git worktree add .worktrees/triple-shot` | [08-opencode-github.md](./08-opencode-github.md) |
| `git worktree list` | Muestra los worktrees registrados. | `git worktree list` | [12-opencode-commads.md](./12-opencode-commads.md) |
| `git worktree remove <ruta>` | Elimina un worktree. Comprueba antes que no necesites sus cambios. | `git worktree remove .worktrees/triple-shot` | [08-opencode-github.md](./08-opencode-github.md) |
| `git restore .` | Descarta cambios locales en archivos rastreados. Revisa antes con `git status` y `git diff`. | `git restore .` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git clean -fdn` / `git clean -fd` | Simula (`-n`) o elimina (`-f`) archivos y directorios no rastreados. Revisa la simulación antes de borrar. | `git clean -fdn` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git remote add origin <url>` | Asocia el repositorio local con un remoto. | `git remote add origin https://github.com/<usuario>/<repositorio>.git` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git branch -M main` | Renombra la rama actual a `main`. | `git branch -M main` | [09-opencode-spec-driven-development.md](./09-opencode-spec-driven-development.md) |
| `git fetch origin` / `git fetch --prune origin` | Actualiza referencias remotas; `--prune` quita referencias remotas que ya no existen. | `git fetch --prune origin` | [08-opencode-github.md](./08-opencode-github.md) |
| `git pull` / `git pull --ff-only` | Actualiza la rama actual con el remoto; `--ff-only` evita crear un merge automático. | `git pull --ff-only` | [08-opencode-github.md](./08-opencode-github.md) |
| `git switch --track origin/<rama>` | Crea una rama local que sigue una rama remota. | `git switch --track origin/opencode/issue1` | [08-opencode-github.md](./08-opencode-github.md) |
| `git push -u origin <rama>` | Publica una rama y configura su seguimiento remoto. | `git push -u origin feature/colors` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `git push` | Publica nuevos commits en el remoto configurado. | `git push` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |

## OpenCode

### CLI y proveedores

| Comando | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `opencode --version` | Comprueba qué versión está instalada. | `opencode --version` | [01_install_opencode_win.md](./01_install_opencode_win.md) |
| `cd <proyecto>` y `opencode` | Abre OpenCode usando la carpeta actual como proyecto. | `cd D:\ruta\proyecto` y luego `opencode` | [01_install_opencode_win.md](./01_install_opencode_win.md) |
| `opencode -c` | Continúa la sesión anterior al iniciar OpenCode. | `opencode -c` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `opencode agent create` | Inicia el asistente para crear un agente personalizado. | `opencode agent create` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
| `opencode github install` | Configura OpenCode Agent para usarlo desde GitHub. | `opencode github install` | [08-opencode-github.md](./08-opencode-github.md) |
| `/connect` | Conecta o autentica un proveedor de modelos. | `/connect` y seleccionar OpenAI u OpenCode Zen | [03_opencode_provider_config.md](./03_opencode_provider_config.md) |
| `/models` | Consulta y selecciona un modelo disponible. | `/models` | [03_opencode_provider_config.md](./03_opencode_provider_config.md) |

### TUI, sesiones e instrucciones

| Comando o atajo | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `/` o `Ctrl + P` | Muestra los comandos disponibles en la TUI. | Escribe `/` para filtrar la lista. | [06_opencode_tui.md](./06_opencode_tui.md) |
| `Ctrl + U` / `Ctrl + W` | Borra el texto desde el cursor hasta el inicio de la línea o la palabra anterior. | `Ctrl + W` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/new` o `Ctrl + X`, luego `N` | Inicia una sesión nueva. | `/new` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `Ctrl + X`, luego `L` | Abre la lista de sesiones anteriores. | `Ctrl + X`, `L` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `Ctrl + R` | Renombra la sesión actual. | `Ctrl + R` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `Ctrl + X`, luego `B` | Muestra u oculta la barra lateral. | `Ctrl + X`, `B` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/compact` | Resume la conversación para reducir el contexto activo. | `/compact` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/fork` | Ejecuta la acción de bifurcar una sesión; la guía lo enumera, pero no detalla el flujo. | `/fork` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/undo` / `Ctrl + X`, luego `U`; `/redo` / `Ctrl + X`, luego `R` | Revierte o restaura el último paso de conversación y, con Git configurado, los cambios en archivos. | `/undo` y luego `/redo` si hace falta | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/init` | Analiza el proyecto y genera instrucciones iniciales en `AGENTS.md`. | `/init` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `/theme` o `/themes` | Abre el selector de temas de la TUI. El nombre cambia entre ejemplos de la documentación. | `/theme` | [04-opencode_global_configs.md](./04-opencode_global_configs.md) |
| `Tab` | Cambia entre los modos `Plan` y `Build`. | Selecciona `Plan`, revisa la propuesta y cambia a `Build`. | [06_opencode_tui.md](./06_opencode_tui.md) |
| `!<comando>` | Ejecuta un comando de terminal desde el prompt y agrega la salida a la conversación. | `!node --version` | [06_opencode_tui.md](./06_opencode_tui.md) |
| `@<archivo>` | Busca o referencia un archivo para incluirlo en la instrucción. | `@README.md` | [06_opencode_tui.md](./06_opencode_tui.md) |

### Comandos personalizados, agentes y SDD

| Comando o instrucción | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `/spec <requerimiento>` | Inicia la creación o refinamiento de una especificación mediante la Skill `spec`. | `/spec Agrega cuatro comportamientos para los fantasmas` | [09-opencode-spec-driven-development.md](./09-opencode-spec-driven-development.md) |
| `/<nombre-comando>` | Ejemplo conceptual de una Skill o comando personalizado; no implica que `/new-powerup` esté configurado. | `/new-powerup` | [09-opencode-spec-driven-development.md](./09-opencode-spec-driven-development.md) |
| `/spec-impl @specs/<archivo>.md` | Implementa una Spec revisada y aprobada mediante la Skill `spec-impl`. | `/spec-impl @specs/01-home-feed.md` | [opencode-daycare/home.md](./opencode-daycare/home.md) |
| `crea spec` en modo `Build` | Solicita generar el archivo de Spec después del análisis en `Plan`. | `/spec` en `Plan`; luego `crea spec` en `Build`. | [opencode-daycare/activate-account.md](./opencode-daycare/activate-account.md) |
| `@spec-acceptance-validator <archivo>` | Invoca el agente personalizado para validar los criterios de aceptación de una Spec. | `@spec-acceptance-validator 01-home-feed.md` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
| `/spec-acceptance-validator <archivo>` | Ejecuta el comando personalizado asociado al agente validador. | `/spec-acceptance-validator 01-home-feed.md` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
| `/worktree <nombre>` | Ejecuta la instrucción personalizada para crear un Git worktree con ese nombre. | `/worktree Triple Shot` | [12-opencode-commads.md](./12-opencode-commads.md) |
| `/oc <solicitud>` en un Issue | Pide a OpenCode Agent que procese una tarea desde GitHub Issues. | `/oc implementa esta funcionalidad` | [08-opencode-github.md](./08-opencode-github.md) |

## Herramientas de entorno y proyecto

### Node.js, npm y Windows

| Comando | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `node --version` / `npm --version` | Comprueba que Node.js y npm estén disponibles. | `node --version` | [01_install_opencode_win.md](./01_install_opencode_win.md) |
| `npm install -g --allow-scripts=opencode-ai opencode-ai` | Instala OpenCode globalmente con npm y permite su script de instalación. | Ejecutarlo en PowerShell. | [01_install_opencode_win.md](./01_install_opencode_win.md) |
| `curl -fsSL https://opencode.ai/install \| bash` | Instalador alternativo documentado para sistemas con Bash; no ejecutarlo directamente en PowerShell. | `curl -fsSL https://opencode.ai/install \| bash` | [01_install_opencode_win.md](./01_install_opencode_win.md) |
| `winget install Warp.Warp` | Intenta instalar Warp con winget; la guía registra que falló la validación del hash en ese equipo. | `winget install Warp.Warp` | [02_install_warp_win.md](./02_install_warp_win.md) |
| `$env:WGPU_BACKEND = "dx12"; & "<ruta>\warp.exe"` | Inicia Warp usando DirectX 12 como backend gráfico. | Configurar la variable y ejecutar `warp.exe`. | [02_install_warp_win.md](./02_install_warp_win.md) |
| `Test-Path <ruta>` / `where.exe warp` | Comprueba que exista un archivo o muestra qué ejecutable se resuelve en `PATH`. | `where.exe warp` | [02_install_warp_win.md](./02_install_warp_win.md) |
| `warp` | Inicia Warp a través del wrapper configurado. | `warp` | [02_install_warp_win.md](./02_install_warp_win.md) |

### Bun, Ollama y herramientas MCP

| Comando | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `bun init` | Inicializa un proyecto Bun. | `bun init` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `bun run <script>` | Ejecuta un script del proyecto, como compilación o desarrollo. | `bun run build` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `bun run src/index.ts [--watch]` | Inicia el programa directamente; `--watch` vuelve a ejecutarlo al detectar cambios. | `bun run src/index.ts --watch` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `bun test` | Ejecuta las pruebas del proyecto con el runner de Bun. | `bun test` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `bun build --compile <entrada> --outfile <salida>` | Compila una aplicación Bun como ejecutable. | `bun build --compile src/index.ts --outfile weather` | [07_opencode-create-automatic.md](./07_opencode-create-automatic.md) |
| `ollama --version` | Comprueba que Ollama esté instalado y disponible. | `ollama --version` | [03_opencode_provider_config.md](./03_opencode_provider_config.md) |
| `ollama launch opencode --model <modelo>` | Inicia OpenCode con un modelo de Ollama. | `ollama launch opencode --model qwen3.5` | [03_opencode_provider_config.md](./03_opencode_provider_config.md) |
| `npx ctx7 setup --mcp --opencode --project` | Configura Context7 como MCP de proyecto para OpenCode. | Ejecutarlo desde la raíz del proyecto. | [10-opencode-mcp.md](./10-opencode-mcp.md) |
| `npx skills add <repositorio>` | Instala Skills desde el repositorio indicado. | `npx skills add klerith/fernando-skills` | [09-opencode-spec-driven-development.md](./09-opencode-spec-driven-development.md) |

### Desarrollo y validación de aplicaciones

| Comando | Para qué sirve | Ejemplo | Documento de referencia |
| --- | --- | --- | --- |
| `npx create-next-app@latest <proyecto>` | Crea una aplicación Next.js. | `npx create-next-app@latest open-daycare` | [10-opencode-mcp.md](./10-opencode-mcp.md) |
| `npm run dev` | Inicia el servidor de desarrollo del proyecto. | `npm run dev` | [10-opencode-mcp.md](./10-opencode-mcp.md) |
| `npx eslint app` | Ejecuta ESLint sobre `app`. | `npx eslint app` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
| `npx tsc --noEmit --incremental false` | Comprueba tipos TypeScript sin generar archivos de salida. | `npx tsc --noEmit --incremental false` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
| `npm run build` | Ejecuta el script de compilación de la aplicación. | `npm run build` | [11-opencode-custom-agent.md](./11-opencode-custom-agent.md) |
