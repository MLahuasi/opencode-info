# Issues y pull requests

## Delegar un cambio desde un issue

### 1. Habilitar Issues

Si la función `Issues` no está habilitada, ve a la configuración del repositorio:

```text
/settings
```

Buscar:

```text
Features
```

y habilitar:

```text
Issues
```

La navegación corresponde a:

```text
Repository
└── Settings
    └── Features
        └── Issues
```

---

### 2. Crear un issue

En la pestaña `Issues`, crea un issue nuevo.

Ejemplo:

```text
Add a title:
Necesitamos una nueva nave

Add a description:
La nueva nave debe ser de color morada y ser dos veces más grande que la nave original.

Al usar esta nueva nave el jugador debe recibir el doble de puntos.
```

---

### 3. Delegar la implementación a OpenCode

Agrega el siguiente comentario al issue:

```text
/oc implementa esta funcionalidad
```

OpenCode comienza a implementar el cambio mediante un workflow de GitHub Actions.

![Ejecución del workflow de OpenCode en GitHub Actions](../assets/01-oc-action.png)

---

## Revisar el pull request generado por OpenCode

Cuando la tarea requiere modificar código, OpenCode puede:

```text
Crear una rama
Implementar los cambios
Crear commits
Crear un pull request
```

OpenCode crea el pull request:

![Pull request creado por OpenCode](../assets/02-oc-pull-request.png)

Para abrirlo, seleccionar el identificador correspondiente.

Por ejemplo:

```text
#2
```

![Enlace al identificador del pull request](../assets/03-oc-pull-request-id.png)

---

## Validar localmente el pull request

Antes de fusionar el pull request, revisa localmente la implementación.

Actualizar referencias:

```bash
git fetch origin
```

Consulta las ramas remotas:

```bash
git branch -r
```

Crea una rama local que siga la rama creada por OpenCode:

```bash
git switch --track origin/opencode/issue1-20260909000525
```

Si la rama local ya existe, cambia a ella:

```bash
git switch opencode/issue1-20260909000525
```

Si la rama remota recibió nuevos commits después de crear el seguimiento, actualízala:

```bash
git pull --ff-only
```

Después se puede:

- Ejecutar la aplicación.
- Revisar los cambios.
- Ejecutar pruebas.
- Validar el comportamiento.

---

## Aprobar, fusionar y limpiar el pull request

Si los cambios son correctos, aprueba y fusiona el pull request desde GitHub.

![Control para fusionar el pull request](../assets/04-oc-pull-request-merge.png)

Después puedes eliminar la rama utilizada por el pull request.

![Opción para eliminar la rama del pull request](../assets/05-delete-branch-pull-request.png)

Actualizar las referencias locales:

```bash
git fetch --prune origin
git branch -r
```

`--prune` elimina referencias locales a ramas remotas que ya no existen.

Si también existe una copia local de la rama y ya no es necesaria:

```bash
git branch -d opencode/issue1-20260909000525
```

Si el pull request está vinculado al issue mediante una palabra clave de cierre, GitHub puede cerrarlo automáticamente al fusionarlo. De lo contrario, cierra el issue manualmente después de verificar la implementación.

![Issue cerrado después de integrar el cambio](../assets/06-close-issue.png)

---

## Resumen del flujo: issue, OpenCode y pull request

```text
GitHub issue
     │
     │ /oc implementa...
     ▼
GitHub Actions
     │
     ▼
OpenCode
     │
     ├── Analiza el issue
     ├── Modifica el proyecto
     └── Crea una rama
              │
              ▼
         Pull request
              │
              ▼
        Revisión local
              │
              ▼
            Merge
              │
              ▼
             main
```

Este flujo permite solicitar trabajo a OpenCode directamente desde GitHub sin ejecutar manualmente el agente desde el entorno local.

---

[← Anterior](05-configuracion-del-agente-en-github.md) | [Temario](../08-opencode-github.md) | [Siguiente →](07-github-actions-personalizados.md)

