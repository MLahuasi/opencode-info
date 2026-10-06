# Workflows personalizados de GitHub Actions con OpenCode

## Objetivo de la automatización

OpenCode también puede ejecutarse desde workflows personalizados.

Antes de implementar un workflow, crea una rama a partir de una versión actualizada de `main`. Así podrás revisarlo mediante un pull request antes de publicarlo.

## Procedimiento

### 1. Preparar la rama de trabajo

Por ejemplo:

```bash
git switch main
git pull --ff-only
git switch -c automation/issue-labeler
```

En este ejemplo se crea una automatización que se ejecuta cuando se crea un nuevo issue.

Los workflows del proyecto pueden consultarse en:

[`.github/workflows/`](https://github.com/MLahuasi/opencode-asteroids/tree/main/.github/workflows)

---

### 2. Solicitar el workflow a OpenCode

En modo `Plan`:

```text
Crea un workflow de GitHub Actions que se ejecute cuando se cree un nuevo issue.

Debe leer el título y el contenido del issue, analizarlo con OpenCode y asignar automáticamente etiquetas según el contenido, por ejemplo: `bug`, `enhancement`, `question`, `documentation`.

También debe enriquecer y reorganizar el contenido del issue para que sea más claro y útil para su revisión, manteniendo siempre la intención, el contexto y la idea original expresada por el usuario.

No uses `GITHUB_TOKEN` ni un token personal de GitHub. Usa la autenticación y el acceso proporcionados por OpenCode para interactuar con GitHub.

No debe crear commits, ramas ni pull requests. La publicación del workflow se realizará manualmente.
```

Después, revisa el plan e impleméntalo en modo `Build`.

---

### 3. Configurar los permisos de OpenCode

Para configurar permisos específicos del proyecto se puede utilizar el archivo:

[`opencode.json`](https://github.com/MLahuasi/opencode-asteroids/blob/main/opencode.json)

ubicado en la raíz del proyecto.

Ejemplo:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "external_directory": {
      "/tmp/**": "allow"
    },
    "bash": "allow"
  }
}
```

En este ejemplo:

```text
external_directory
```

permite trabajar sobre `/tmp/**`.

Y:

```text
bash
```

permite ejecutar comandos mediante shell.

> Estos son permisos de OpenCode; no deben confundirse con los permisos del workflow de GitHub Actions.

Solo deben habilitarse los permisos realmente necesarios para el proyecto.

---

### 4. Configurar los permisos del workflow

El workflow tiene su propio sistema de permisos, independiente de los permisos configurados en `opencode.json`.

Para un workflow que modifica issues pueden requerirse permisos como:

```yaml
permissions:
  id-token: write
  issues: write
```

Conceptualmente:

```text
id-token: write
    │
    └── permite el mecanismo de autenticación utilizado por OpenCode

issues: write
    │
    ├── modificar etiquetas
    ├── modificar contenido
    └── publicar información en el issue
```

Los permisos de GitHub Actions se definen dentro del workflow correspondiente en:

[`.github/workflows/`](https://github.com/MLahuasi/opencode-asteroids/tree/main/.github/workflows)

---

### 5. Definir los eventos de ejecución

OpenCode no tiene que ejecutarse únicamente como respuesta a comentarios `/oc`.

También puede utilizarse con eventos automáticos como:

```text
Issue creado
Issue modificado
Pull request creado
Pull request actualizado
Ejecución manual
Ejecución programada
```

Por ejemplo:

```yaml
on:
  issues:
    types: [opened]
```

Cuando el workflow se ejecuta automáticamente, debe incluir un `prompt` que indique a OpenCode qué hacer. En este caso no existe un comentario `/oc` que proporcione la instrucción.

Por ejemplo:

```text
Analiza el issue recién creado.

Clasifícalo y agrega las etiquetas correspondientes.

Reorganiza su contenido para mejorar su claridad sin modificar la intención original.
```

---

### 6. Publicar el workflow

Una vez implementado con `Build` y revisado el resultado, confirma que estás en la rama de trabajo y registra el workflow:

```bash
git status
git add .github/workflows/<workflow>.yml
git commit -m "Add issue labeler workflow"
git push -u origin automation/issue-labeler
```

Abre un pull request para integrar el cambio en `main`.

Los workflows publicados pueden consultarse directamente en:

[`.github/workflows/`](https://github.com/MLahuasi/opencode-asteroids/tree/main/.github/workflows)

---

### 7. Probar el etiquetado automático

Después de fusionar el workflow en la rama predeterminada, crea un issue en GitHub.

Ejemplo:

```text
Add a title:
Incrementar el tamaño de la pantalla

Add a description:
La pantalla del juego debe ser duplicada
```

GitHub Actions ejecuta el workflow, que invoca a OpenCode para analizar el issue y asignarle una etiqueta apropiada.

Por ejemplo:

```text
enhancement
```

---

### 8. Implementar posteriormente la solicitud del issue

La clasificación automática del issue y su implementación pueden mantenerse como procesos independientes.

El primer workflow puede encargarse únicamente de:

```text
Analizar
Reorganizar
Etiquetar
```

Después, cuando decidas implementar la funcionalidad, agrega el siguiente comentario:

```text
/oc implementa lo solicitado
```

OpenCode podrá iniciar el proceso de implementación y crear el pull request correspondiente.

---

#### Validar el pull request

Aplica el procedimiento de [validación local](06-issues-y-pull-requests.md#validar-localmente-el-pull-request) y de [aprobación y fusión](06-issues-y-pull-requests.md#aprobar-fusionar-y-limpiar-el-pull-request) descrito en el documento anterior. Sustituye la rama de ejemplo `opencode/issue6-20260909171116` por la rama generada para el issue actual.

---

[← Anterior](06-issues-y-pull-requests.md) | [Temario](../08-opencode-github.md) | [Siguiente →](08-referencias.md)
