# Comandos personalizados en OpenCode

Los comandos personalizados convierten instrucciones frecuentes en atajos que se ejecutan desde OpenCode con `/`. Se pueden definir en el archivo de configuración del proyecto o como archivos Markdown. Esta guía presenta ambos métodos y usa la creación de un Git worktree como ejemplo.

Consulta la [documentación oficial de comandos](https://opencode.ai/docs/commands/) para conocer las opciones disponibles.

## Temario

1. [Definir un comando en la configuración](#1-definir-un-comando-en-la-configuración)
2. [Crear un comando Markdown](#2-crear-un-comando-markdown)
3. [Ejecutar y verificar el comando de worktree](#3-ejecutar-y-verificar-el-comando-de-worktree)

---

## 1. Definir un comando en la configuración

Para un comando breve, agrega una entrada bajo `command` en el archivo de configuración del proyecto. El nombre de la propiedad (`spec`) determina el nombre que se escribe después de `/`; `template` contiene la instrucción y puede incluir `$ARGUMENTS` para recibir texto adicional.

En el ejemplo, el comando se define en [opencode.json](https://github.com/MLahuasi/opencode-daycare/blob/main/opencode.json):

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "command": {
    "spec": {
      "description": "Crea especificaciones de pantallas y funcionalidades",
      "template": "Carga y sigue la skill spec en .agents/skills/spec/SKILL.md.\n\nFeature: $ARGUMENTS"
    }
  }
}
```

Integra la propiedad `command` en la configuración existente, sin reemplazar otras opciones del proyecto. Una vez cargado, el comando aparece al escribir `/` y se ejecuta, por ejemplo, como `/spec nueva funcionalidad`.

![](./assets/21-opencode-command-config-json.png)

---

## 2. Crear un comando Markdown

Los archivos Markdown permiten guardar instrucciones más extensas y compartirlas con el proyecto. Para un comando nuevo, usa `.opencode/commands/`; el nombre del archivo determina el nombre del comando. OpenCode todavía reconoce la carpeta singular `.opencode/command/`, usada por el ejemplo existente, pero recomienda la plural para archivos nuevos ([documentación y rutas](https://opencode.ai/docs/commands/)).

En modo `Plan`, solicita el diseño del comando y sus límites. Después, en modo `Build`, pide implementar ese plan. Por ejemplo:

```text
Crea un comando personalizado de OpenCode a nivel de proyecto.

Debe ejecutarse mediante `/worktree` y utilizar:

git worktree add .worktrees/<nombre-del-worktree>

El comando recibe un argumento que puede contener espacios. Analiza el argumento según su significado y genera un nombre válido para el worktree.

Solo debe ejecutar una vez el comando indicado para crear el worktree.
```

El archivo del ejemplo está en [`.opencode/command/worktree.md`](https://github.com/MLahuasi/opencode-asteroids/blob/main/.opencode/command/worktree.md). La captura muestra su configuración:

![](./assets/18-opencode-command-custom.png)

La instrucción acepta argumentos mediante `$ARGUMENTS`, deriva un nombre seguro en `kebab-case` y solicita ejecutar una sola vez:

```bash
git worktree add .worktrees/<nombre-del-worktree>
```

---

## 3. Ejecutar y verificar el comando de worktree

Desde OpenCode, proporciona el nombre del worktree como argumento:

```text
/worktree Triple Shot
```

El comando puede derivar `triple-shot` y ejecutar:

```bash
git worktree add .worktrees/triple-shot
```

Después, verifica los worktrees del repositorio desde la terminal:

```bash
git worktree list
```

La salida incluye la ruta, el commit y la rama de cada worktree. Por ejemplo:

```text
.../asteroids/.worktrees/point  0022e39 [point]
```

OpenCode recarga automáticamente los cambios en los archivos de comandos y configuración, por lo que normalmente no hace falta reiniciarlo. Si el comando no aparece, comprueba su nombre y que el archivo esté en una carpeta reconocida.
