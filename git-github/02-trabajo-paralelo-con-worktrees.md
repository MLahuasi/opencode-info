# Trabajo paralelo con worktrees de Git

## Qué es un worktree

Un worktree es un directorio de trabajo adicional vinculado al mismo repositorio y asociado normalmente a una rama distinta. Permite tener varias copias de trabajo sin clonar el repositorio varias veces.

Esto resulta especialmente útil con OpenCode, porque se puede ejecutar una sesión independiente dentro de cada worktree.

```text
Repositorio
    │
    ├── Worktree A (basado en main) ──► OpenCode #1
    ├── Worktree B (basado en main) ──► OpenCode #2
    └── Worktree C (basado en main) ──► OpenCode #3
```

Las sesiones pueden trabajar simultáneamente sin cambiar constantemente de rama.

---

## Procedimiento

### 1. Preparar el repositorio

Antes de crear los worktrees, comprueba que el repositorio principal esté en `main`, actualizado y sin cambios pendientes. Las nuevas ramas se crearán a partir del commit actualmente activo.

Añade `.worktrees/` a `.gitignore` antes de crear los directorios de trabajo.

### 2. Crear los worktrees

En este ejemplo se crearán worktrees para las siguientes funcionalidades:

```text
triple-shot
skins-system
shield
```

Ejecuta:

```bash
git worktree add .worktrees/triple-shot
git worktree add .worktrees/skins-system
git worktree add .worktrees/shield
```

Si las ramas aún no existen, Git crea automáticamente una rama con el nombre del último segmento de cada ruta.

Por ejemplo:

```text
Directorio: .worktrees/triple-shot
Rama: triple-shot
```

La estructura será similar a:

```text
asteroids/
│
├── .git/
├── .worktrees/
│   ├── triple-shot/
│   ├── skins-system/
│   └── shield/
└── ...
```

Cada directorio contiene su propia copia de trabajo del proyecto.

---

### 3. Ejecutar una sesión de OpenCode en cada worktree

Abre una terminal en cada worktree.

Por ejemplo:

```text
Terminal 1
.worktrees/triple-shot

Terminal 2
.worktrees/skins-system

Terminal 3
.worktrees/shield
```

En cada terminal, ejecuta:

```bash
opencode
```

Las sesiones de OpenCode pueden ejecutarse simultáneamente porque trabajan en worktrees y ramas independientes.

```text
                    Repositorio Git
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
     triple-shot      skins-system     shield
          │               │               │
          ▼               ▼               ▼
      OpenCode #1      OpenCode #2      OpenCode #3
          │               │               │
      Plan/Build      Plan/Build      Plan/Build
```

---

### 4. Planificar las funcionalidades

Cada sesión puede planificar su funcionalidad independientemente.

#### Rama `triple-shot`

En modo `Plan`:

```text
Planea la implementación de triple shot: por 5 segundos, el personaje dispara 3 veces en línea recta.
```

#### Rama `skins-system`

En modo `Plan`:

```text
Planea la implementación de un sistema de skins: poder cambiar la apariencia de la nave.
```

#### Rama `shield`

En modo `Plan`:

```text
Planea la implementación de un escudo: un escudo que protege a la nave de los proyectiles enemigos.
```

---

### 5. Implementar los planes

En cada sesión, cambia al modo `Build` y solicita:

```text
Implementa el plan
```

Cada sesión de OpenCode modificará únicamente la copia de trabajo correspondiente a su worktree.

---

### 6. Registrar los cambios

Una vez implementada y revisada cada funcionalidad, registrar los cambios.

#### Opción A: crear los commits manualmente

Desde cada worktree:

```bash
git add .
git commit -m "Add ..."
```

Por ejemplo:

```bash
git add .
git commit -m "Add triple shot"
```

Repite el mismo proceso en cada worktree.

---

#### Opción B: solicitar los commits a OpenCode

También puedes solicitar lo siguiente en cada sesión de OpenCode:

```text
Ejecuta el commit de los cambios
```

OpenCode puede revisar los cambios y ejecutar el commit sobre la rama correspondiente.

---

[← Anterior](01-flujo-local-con-ramas.md) | [Temario](../08-opencode-github.md) | [Siguiente →](03-integracion-de-worktrees.md)

