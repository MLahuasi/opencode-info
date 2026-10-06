# Limpieza de worktrees y ramas

## Comprobar el estado del repositorio

Antes de limpiar el repositorio, conviene comprobar qué ramas y worktrees siguen existiendo.

### Ver las ramas

```bash
git branch
```

Ejemplo:

```text
* main
+ shield
+ skins-system
+ triple-shot
  union-worktree
```

Donde:

```text
*  rama activa
+  rama utilizada actualmente por otro worktree
```

---

### Ver los worktrees

```bash
git worktree list
```

Ejemplo:

```text
.../asteroids                           0022e39 [main]
.../asteroids/.worktrees/shield         098d4f2 [shield]
.../asteroids/.worktrees/skins-system   b3a9552 [skins-system]
.../asteroids/.worktrees/triple-shot    97d471a [triple-shot]
```

Esta información permite conocer:

- Directorio del worktree.
- Commit actual.
- Rama asociada.

---

## Eliminar los worktrees

Una vez integradas las funcionalidades:

```bash
git worktree remove .worktrees/shield
git worktree remove .worktrees/skins-system
git worktree remove .worktrees/triple-shot
```

Comprobar:

```bash
git worktree list
```

Usa `git worktree remove` en lugar de eliminar manualmente los directorios.

---

## Eliminar las ramas temporales

Después de eliminar los worktrees:

```bash
git branch -d shield
git branch -d skins-system
git branch -d triple-shot
git branch -d union-worktree
```

Las opciones tienen el siguiente comportamiento:

```text
-d  Elimina la rama si Git considera que ya fue integrada.

-D  Fuerza la eliminación aunque existan commits exclusivos.
```

Siempre que sea posible, utiliza `-d`, que impide eliminar una rama que Git no considere integrada. Usa `-D` solo cuando quieras descartar deliberadamente commits no integrados.

```bash
git branch -d <rama>
```

## Resumen del flujo local con OpenCode y worktrees

```text
                         Git / main
                             │
              Crear funcionalidades independientes
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          ▼                  ▼                  ▼
     triple-shot        skins-system          shield
          │                  │                  │
      worktree #1         worktree #2        worktree #3
          │                  │                  │
     OpenCode #1         OpenCode #2        OpenCode #3
          │                  │                  │
        Plan               Plan               Plan
          │                  │                  │
        Build              Build              Build
          │                  │                  │
        Commit             Commit             Commit
          │                  │                  │
          └──────────────────┬──────────────────┘
                             │
                             ▼
                       union-worktree
                             │
                 Merge de las ramas de funcionalidades
                             │
                       OpenCode Plan
                             │
                       OpenCode Build
                             │
                      Prueba conjunta
                             │
             Commit si hay ajustes adicionales
                             │
                            Merge
                             │
                             ▼
                            main
                             │
                    Eliminar worktrees
                             │
                    Eliminar ramas usadas
```

## Automatizar el proceso con un comando de OpenCode

Este proceso puede automatizarse mediante un [comando personalizado de OpenCode](../12-opencode-commads.md).

---

[← Anterior](03-integracion-de-worktrees.md) | [Temario](../08-opencode-github.md) | [Siguiente →](05-configuracion-del-agente-en-github.md)

