# Integrar OpenCode con Git y GitHub

Repositorio utilizado en los ejemplos: [OpenCode Asteroids](https://github.com/MLahuasi/opencode-asteroids).

OpenCode puede integrarse con Git y GitHub para planificar, implementar, revisar y versionar cambios dentro de un proyecto.

Este temario utiliza un juego de Asteroids para mostrar diferentes formas de integrar estas herramientas.

## Temario

### Desarrollo local con Git

1. [Desarrollo local con ramas](./git-github/01-flujo-local-con-ramas.md) — crear una rama, planificar con OpenCode, implementar, crear commits e integrar cambios en `main`.
2. [Trabajo paralelo con worktrees de Git](./git-github/02-trabajo-paralelo-con-worktrees.md) — trabajar con varias ramas, worktrees y sesiones independientes de OpenCode.
3. [Integración de las ramas de los worktrees](./git-github/03-integracion-de-worktrees.md) — fusionar funcionalidades, resolver conflictos, revisar la integración y combinarla con `main`.
4. [Limpieza de worktrees y ramas](./git-github/04-limpieza-de-worktrees-y-ramas.md) — consultar y eliminar worktrees y ramas ya integrados.

### GitHub y OpenCode

5. [Configuración del agente de OpenCode en GitHub](./git-github/05-configuracion-del-agente-en-github.md) — instalar el agente, configurar la aplicación, los secretos y el workflow.
6. [Issues y pull requests](./git-github/06-issues-y-pull-requests.md) — delegar tareas desde issues, revisar pull requests y validar cambios localmente.
7. [Workflows personalizados de GitHub Actions](./git-github/07-github-actions-personalizados.md) — crear automatizaciones, configurar permisos, usar eventos y publicar workflows.
8. [Referencias](./git-github/08-referencias.md) — repositorio de ejemplo, archivos importantes, documentación oficial y rutas de configuración.
9. [Aprobación semiautomática de Specs](./git-github/09-spec-revision.md) — ejecutar desde GitHub un validador de Specs mediante OpenCode Agent, workflows, secretos e issues.

## Modelo general

```text
Git
│
├── Control de versiones local
├── Ramas
├── Commits
├── Merges
└── Worktrees

GitHub
│
├── Repositorio remoto
├── Issues
├── Pull Requests
└── GitHub Actions

OpenCode
│
├── Planifica cambios
├── Implementa cambios
├── Puede trabajar sobre ramas y worktrees
└── Puede ejecutarse desde GitHub Actions
```

## Cómo recorrer el temario

Para el desarrollo local, sigue los documentos 1 a 4. Para trabajar desde GitHub, configura primero el agente en el documento 5 y continúa con los issues y pull requests del documento 6. El documento 7 explica cómo crear workflows personalizados. El documento 8 reúne las referencias utilizadas por todo el temario. El documento 9 explica cómo ejecutar de forma semiautomática el validador de Specs.

## Flujo completo: OpenCode, Git y GitHub

```text
                             OpenCode
                                │
           ┌────────────────────┴────────────────────┐
           │                                         │
           ▼                                         ▼
      Desarrollo local                         Desarrollo GitHub
           │                                         │
           ▼                                         ▼
         Git                                   GitHub Issue
           │                                         │
     ┌─────┴─────┐                                   │
     │           │                                   ▼
   Ramas      Worktrees                         GitHub Actions
     │           │                                   │
     │       Varias sesiones                         ▼
     │        OpenCode                             OpenCode
     │           │                                   │
     └─────┬─────┘                                   │
           │                                         ▼
           ▼                                   Nueva rama
         Commit                                      │
           │                                         ▼
           ▼                                   Pull Request
         Merge                                       │
           │                                         ▼
           └──────────────────► main ◄──────────────Merge
```

OpenCode puede utilizarse en dos escenarios complementarios:

```text
LOCAL

OpenCode + Git

Planificar, implementar y versionar cambios directamente desde el entorno de desarrollo.


REMOTO

OpenCode + GitHub Actions

Procesar issues, automatizar tareas e implementar funcionalidades desde GitHub.
```

Git sigue siendo responsable del control de versiones y del aislamiento de los cambios, mientras que GitHub proporciona colaboración, automatización y revisión mediante issues y pull requests.

OpenCode actúa como agente sobre ambos flujos.

---

[← Anterior](07_opencode-assisted-development.md) | [Temario](README.md) | [Siguiente →](09-opencode-spec-driven-development.md)
