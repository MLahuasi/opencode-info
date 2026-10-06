# Integración de las ramas de los worktrees

## Objetivo y estrategia de integración

Después de completar `triple-shot`, `skins-system` y `shield`, utiliza una rama temporal de integración.

Esto permite probar las tres funcionalidades juntas antes de integrarlas en `main`.

> Crear varios worktrees no integra sus cambios automáticamente. Cada worktree está asociado a una rama; para que `union-worktree` contenga las funcionalidades, integra en ella las ramas completadas con Git. OpenCode puede ayudar a revisar o resolver conflictos después de esa integración.

```text
triple-shot ────┐
                │
skins-system ───┼──► union-worktree ───► main
                │
shield ─────────┘
```

## Procedimiento de integración

### 1. Crear la rama de integración

Desde `main`:

```bash
git switch -c union-worktree
```

Alternativamente:

```bash
git checkout -b union-worktree
```

---

### 2. Abrir OpenCode en la rama de integración

Comprueba la rama activa:

```bash
git branch --show-current
```

OpenCode debe ejecutarse sobre la siguiente rama:

```text
union-worktree
```

---

### 3. Integrar las ramas y planificar la revisión

Primero, desde `union-worktree`, integra las ramas que contienen los commits de cada funcionalidad:

```bash
git merge triple-shot
git merge skins-system
git merge shield
```

Si aparece un conflicto, resuélvelo antes de iniciar el siguiente merge. Añade los archivos resueltos al área de preparación y completa el merge. Después, abre OpenCode en esta rama y solicita en modo `Plan` una revisión conjunta de la integración:

```text
Revisa la integración de las funcionalidades triple-shot, skins-system y shield en esta rama.
Comprueba que funcionen conjuntamente, identifica conflictos de comportamiento y propón los ajustes necesarios.
No elimines worktrees ni ramas.
```

Cada `git merge` incorpora en `union-worktree` los commits de la rama correspondiente. Según el historial, Git puede realizar un avance rápido o crear un commit de merge. Si OpenCode propone ajustes adicionales, revísalos y registra esos cambios después de validarlos.

---

### 4. Implementar la integración

Las ramas ya se han fusionado mediante Git. Si el plan propone ajustes adicionales, impleméntalos en `union-worktree` con el modo `Build`:

```text
Implementa el plan
```

Después, revisa y prueba el resultado. Crea un commit adicional únicamente si la revisión produjo cambios que aún no están registrados.

---

## Revisar y registrar los ajustes

### 1. Confirmar la rama activa

Cambia a la rama de integración:

```bash
git switch union-worktree
```

Una salida posible:

```text
M       README.md
M       game.js
Switched to branch 'union-worktree'
```

La letra `M` indica que existen modificaciones locales. También puedes comprobar la rama activa con:

```bash
git branch --show-current
```

### 2. Revisar el estado del repositorio

```bash
git status
```

Esto permite verificar qué archivos fueron modificados.

### 3. Comprender el área de preparación

Git registra los cambios preparados en el historial del repositorio mediante el siguiente flujo:

```text
Working Directory
      │
      │ git add .
      ▼
Staging Area
      │
      │ git commit
      ▼
Repository / Commit
```

### 4. Preparar los cambios

Si OpenCode realizó ajustes adicionales después de completar los merges, agrégalos al área de preparación:

```bash
git add .
```

Verifica nuevamente el estado:

```bash
git status
```

Ejemplo:

```text
Changes to be committed:

    modified: README.md
    modified: game.js
```

Los archivos están preparados para el próximo commit.

### 5. Revisar las advertencias sobre LF y CRLF

En Windows puede aparecer:

```text
LF will be replaced by CRLF
```

Esto corresponde a los finales de línea utilizados por diferentes sistemas operativos:

```text
LF      Linux / macOS
CRLF    Windows
```

No significa que `git add` haya fallado.

### 6. Crear un commit para los ajustes adicionales

Este paso solo se aplica si OpenCode realizó ajustes después de completar los merges. Los conflictos de un merge deben resolverse y registrarse al completar ese merge.

```bash
git commit -m "Resolve integration adjustments"
```

Ejemplo:

```text
[union-worktree 0022e39] Add union worktrees
3 files changed, 261 insertions(+), 53 deletions(-)
create mode 100644 .gitignore
```

---

## Integrar la rama temporal en `main`

Cambiar a:

```bash
git switch main
```

Realizar el merge:

```bash
git merge union-worktree
```

---

[← Anterior](02-trabajo-paralelo-con-worktrees.md) | [Temario](../08-opencode-github.md) | [Siguiente →](04-limpieza-de-worktrees-y-ramas.md)

