# Desarrollo local con ramas

## Objetivo y flujo de trabajo

El objetivo es implementar una nueva funcionalidad en una rama aislada y después integrarla en `main`. En este ejemplo se implementa un nuevo power-up llamado `velocidad`.

El flujo de trabajo será el siguiente:

```text
main
  │
  └── rama de funcionalidad
          │
          ├── OpenCode / Plan
          ├── OpenCode / Build
          └── Commit
                  │
                  ▼
                Merge
                  │
                  ▼
                 main
```

## Procedimiento

### 1. Crear la rama

Crea una rama y cambia a ella:

```bash
git switch -c 01-powerup-velocity
```

Alternativamente:

```bash
git checkout -b 01-powerup-velocity
```

La funcionalidad queda aislada de `main` mientras se desarrolla.

---

### 2. Planificar la funcionalidad con OpenCode

Abre OpenCode sobre el repositorio y selecciona el modo `Plan`.

Solicitar:

```text
Agrega una nueva característica (feature) o power-up al juego que se llamará `velocidad`, cuyo objetivo es permitirle al personaje moverse al doble de velocidad durante cinco segundos.
```

OpenCode puede hacer preguntas adicionales para definir correctamente el comportamiento antes de modificar el código. Por ejemplo:

> - ¿Cómo debería obtenerse el power-up `velocidad`?
> - ¿Qué debe duplicarse durante los 5 segundos?
> - Si se recoge otro mientras está activo, ¿cómo se actualiza la duración?
> - ¿Con qué frecuencia debería aparecer?
> - ¿Cuánto dura un power-up sin recoger?

El modo `Plan` permite definir primero las reglas de la funcionalidad y reducir decisiones ambiguas durante la implementación.

---

### 3. Implementar el plan

Cambia OpenCode al modo `Build` y solicita:

```text
Implementa el plan
```

OpenCode implementará los cambios definidos durante la planificación.

---

### 4. Revisar y crear el commit

Antes de crear el commit, revisa el estado del repositorio:

```bash
git status
```

Si los cambios son correctos, agrégalos al área de preparación y crea el commit:

```bash
git add .
git commit -m "Add power-up velocidad"
```

Los cambios quedan registrados en la rama:

```text
01-powerup-velocity
```

---

### 5. Integrar la funcionalidad en `main`

Cambia a la rama `main`:

```bash
git switch main
```

Integrar la rama:

```bash
git merge 01-powerup-velocity
```

El flujo utilizado fue:

```text
main
  │
  ├── 01-powerup-velocity
  │       │
  │       ├── Plan
  │       ├── Build
  │       └── Commit
  │
  └────── Merge ──────► main
```

> En proyectos colaborativos suele utilizarse un pull request antes de integrar los cambios en `main`. Aquí se usa un merge directo para simplificar el ejemplo.

---

[Temario](../08-opencode-github.md) | [Siguiente →](02-trabajo-paralelo-con-worktrees.md)
