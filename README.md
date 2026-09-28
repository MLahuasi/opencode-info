# Temario: OpenCode, herramientas y desarrollo asistido

Este repositorio reúne guías, conceptos y ejemplos para trabajar con OpenCode. El recorrido cubre la instalación y configuración, el contexto de trabajo, la automatización, Git y GitHub, MCP, agentes y Spec Driven Development (SDD). Los proyectos de ejemplo —incluido el laboratorio OpenDayCare— ilustran la aplicación práctica de esos temas.

## 1. Preparar el entorno

1. [Instalar OpenCode en Windows](./01_install_opencode_win.md) — requisitos e instalación inicial.
2. [Configurar Warp en Windows](./02_install_warp_win.md) — instalación y preparación de una terminal para trabajar con OpenCode.
3. [Configurar proveedores en OpenCode](./03_opencode_provider_config.md) — conectar OpenCode Zen, OpenAI y Ollama.

## 2. Entender la configuración y el uso diario

4. [Configuración global y TUI](./04-opencode_global_configs.md) — ubicaciones de configuración y datos, y ajustes de OpenCode.
5. [Contexto en conversaciones con LLM](./05_opencode-context.md) — qué significa que un modelo sea stateless y qué información compone el contexto.
6. [Interfaz TUI de OpenCode](./06_opencode_tui.md) — iniciar y utilizar OpenCode desde una terminal.

## 3. Planificar, automatizar y colaborar

7. [Creación automática usando OpenCode](./07_opencode-create-automatic.md) — preparar un proyecto, documentar requisitos y recorrer el flujo de planificación e implementación.
8. [Integrar OpenCode con Git y GitHub](./08-opencode-github.md) — ramas, commits, merges, worktrees y colaboración mediante GitHub.
9. [Spec Driven Development (SDD)](./09-opencode-spec-driven-development.md) — trabajar con specs, Skills, agentes y Git para implementar cambios de forma controlada.
10. [MCPs y agentes personalizados](./10-opencode-mcp.md) — configurar MCP y conocer su relación con agentes y herramientas.
11. [Agentes personalizados](./11-opencode-custom-agent.md) — definir subagentes especializados, instrucciones y permisos.
12. [Comandos personalizados](./12-opencode-commads.md) — crear comandos reutilizables para automatizar tareas frecuentes.

## 4. Referencia de modelos

13. [Modelos OpenAI](./modelos.openaai.md) — orientación de modelos por capacidad y nivel de esfuerzo (Low, Medium y High) para distintos tipos de tareas.

## 5. Laboratorio: aplicación OpenDayCare

Estos documentos reúnen ejemplos de aplicación de los temas anteriores en un proyecto. OpenDayCare funciona como laboratorio dentro del material, junto con los demás ejemplos y guías.

14. [Home y feed](./opencode-daycare/home.md) — especificar la pantalla principal, sus referencias visuales y sus criterios de aceptación.
15. [Listado y perfiles de niños](./opencode-daycare/kids.md) — dividir el trabajo en specs, organizar mocks, navegación, datos y criterios de aceptación.
16. [Agregar y editar niños](./opencode-daycare/add-edit-kid.md) — formularios, validación, salas, alergias y almacenamiento temporal en mocks JSON.
17. [Organizar la arquitectura](./opencode-daycare/organice-architecture.md) — planificar una reorganización incremental, implementar por pasos y preparar la integración.
18. [Login y activación de cuenta](./opencode-daycare/login-activate.md) — tipos y flujos iniciales de acceso y activación.
19. [Configurar el correo de invitación](./opencode-daycare/email.md) — integrar un servicio de correo y definir una plantilla acorde a la marca.
20. [Vincular padres y completar la activación](./opencode-daycare/activate-account.md) — flujo de invitación, generación de código, envío de correo y acceso.
21. [Crear y actualizar publicaciones](./opencode-daycare/create-update-post.md) — publicaciones, selección de imágenes, persistencia temporal y feed para familias.
