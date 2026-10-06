# Versionar y colaborar con Git y GitHub

Este documento resume cómo relacionar el flujo de OpenCode con Git y GitHub. Los procedimientos completos se encuentran en el [capítulo de Git y GitHub](../08-opencode-github.md).

> **Ejemplo:** los nombres de ramas, mensajes de commit, rutas y comandos representan un flujo didáctico. Revisa siempre el estado y las reglas del repositorio antes de ejecutarlos.

## 1. Preparar el repositorio

Este flujo presupone que el proyecto ya es un repositorio Git con al menos un commit. Si se trata de un proyecto nuevo:

```bash
git init
```

Antes de implementar una funcionalidad importante crea una rama:

```bash
git switch -c feature/colors
```

## 2. Revisar y registrar los cambios

Después de implementar y probar una funcionalidad:

```bash
git status
git diff
git add <rutas-revisadas>
git diff --cached
git commit -m "Implement feature"
```

Revisa los cambios antes de preparar el commit. Evita usar `git add .` sin comprobar que no incluya secretos, binarios o archivos no solicitados.

## 3. Descartar cambios con seguridad

Para restaurar cambios no preparados del directorio de trabajo:

```bash
git restore .
```

Para quitar cambios del staging, conservando su contenido:

```bash
git restore --staged .
```

Para restaurar el índice y el directorio de trabajo desde `HEAD`:

```bash
git restore --source=HEAD --staged --worktree .
```

El último comando es destructivo. Revisa `git status` y `git diff` antes de ejecutarlo.

Para simular y eliminar archivos no rastreados:

```bash
git clean -fdn
git clean -fd
```

`git clean -fd` elimina archivos y directorios no rastreados. Comprueba primero la simulación.

## 4. Publicar y colaborar

Vincula el repositorio local con un remoto cuando corresponda:

```bash
git remote add origin https://github.com/<usuario>/<repositorio>.git
git branch -M main
git push -u origin main
```

Para integrar cambios localmente:

```bash
git switch main
git merge feature/colors
git branch -d feature/colors
```

En proyectos colaborativos es preferible publicar la rama y abrir un Pull Request. Para revisar, validar, aprobar, fusionar y limpiar un Pull Request, sigue [Issues y pull requests](../git-github/06-issues-y-pull-requests.md).

Para trabajar con varias sesiones aisladas de OpenCode, consulta [Trabajo paralelo con worktrees](../git-github/02-trabajo-paralelo-con-worktrees.md).

---

[← Anterior](04-validar-con-pruebas.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente →](06-automatizar-compilaciones-y-publicaciones.md)
