# Crear comandos personalizados de OpenCode

[OpenCode - Commands](https://opencode.ai/docs/commands/)

Los `comandos personalizados` permiten convertir instrucciones frecuentes en comandos reutilizables dentro de `OpenCode`.

**Ejemplo**: Crear un comando para automatizar la creación de un `Git Worktree`:

```text
/worktree
```

---

## FORMAS DE CREAR UN COMANDO

### Modificar archivo [opencode.json](../open-daycare/opencode.json)

Se puede agregar directamente, se especifica desde que skill se debe ejecutar:

```json
"command": {
  "spec": {
    "description": "Crea especificaciones de pantallas y funcionalidades",
    "template": "Carga y sigue la skill spec en .agents/skills/spec/SKILL.md.\n\nFeature: $ARGUMENTS"
  }
}
```

Una vez configurada aparecen como comandos en `OpenCode` al colocar `/`

![](./assets/21-opencode-command-config-json.png)

---

### Solicitar la creación del comando a OPEN CODE

- En modo `Plan` solicitar algo semejante a:

```text
Crea un comando personalizado de OpenCode a nivel de proyecto.

Debe ejecutarse mediante `/worktree` y utilizar:

git worktree add .worktrees/<nombre-del-worktree>

El comando recibe un argumento que puede contener espacios. Analiza el argumento según su significado y genera un nombre válido para el worktree.

Solo debe ejecutarse el comando indicado para crear el worktree.
```

- En modo `Build` solicitar la implementación del `Plan`.

- Para este ejemplo crea:

[`.opencode/command/worktree.md`](https://github.com/MLahuasi/opencode-asteroids/blob/main/.opencode/command/worktree.md)

![](./assets/18-opencode-command-custom.png)

- En su confuguración se aprecia que ejecuta:

```bash
git worktree add .worktrees/<nombre-del-worktree>
```

- El comando:

  > - Está disponible como `/worktree`.
  > - Acepta argumentos con espacios mediante `$ARGUMENTS`.
  > - Deriva un nombre seguro en `kebab-case`.
  > - Ejecuta únicamente una vez:

- Después de crear o modificar el comando se debe reiniciar OpenCode para que cargue la nueva configuración.

- Por ejemplo (se ejecuta dentro de `OpenCode`):

```text
/worktree Triple Shot
```

> - Puede derivar a:

```text
triple-shot
```

> - Se ejecuta:

```bash
git worktree add .worktrees/triple-shot
```

---

## Verificar el Worktree

Ejecutar desde `console` dentro del directorio en donde se creó el comando:

```bash
git worktree list
```

Ejemplo:

```text
.../asteroids/.worktrees/point  0022e39 [point]
```
