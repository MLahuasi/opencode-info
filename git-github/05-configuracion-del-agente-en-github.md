# Configuración del agente de OpenCode en GitHub

## Descripción general de la integración

Documentación oficial:

[OpenCode - GitHub](https://opencode.ai/docs/github/)

La integración permite utilizar OpenCode desde:

```text
GitHub Issues
Pull requests
GitHub Actions
```

Conceptualmente:

```text
GitHub
   │
   ▼
Issue / pull request
   │
   ▼
GitHub Actions
   │
   ▼
OpenCode
   │
   ├── analiza
   ├── responde
   ├── implementa
   └── puede crear pull requests
```

---

## Configurar el agente de OpenCode

### 1. Preparar la rama e instalar el agente

Desde una terminal situada en el repositorio, actualiza `main` y crea una rama dedicada para revisar el workflow generado por el instalador:

```bash
git switch main
git pull --ff-only
git switch -c chore/opencode-github-agent
```

Ejecuta el instalador desde la terminal, fuera de una sesión interactiva de OpenCode:

```bash
opencode github install
```

Después de completar la instalación, reinicia OpenCode si la sesión actual no reconoce la nueva configuración.

Durante el proceso se solicita:

1. Autorizar OpenCode en GitHub.
2. Seleccionar el proveedor.
3. Seleccionar el modelo.
4. Configurar el workflow necesario.

Una salida posible:

```text
┌    Install GitHub agent
│
◇  GitHub app already installed
│
◇  Select provider
│  OpenAI
│
◇  Select model
│  GPT-5.6 Luna
│
◆  Added workflow file: ".github/workflows/opencode.yml"
│
└  Next steps:

    1. Commit the `.github/workflows/opencode.yml` file and push
    2. Add the following secrets in org or repo settings

       - OPENAI_API_KEY

    3. Go to a GitHub issue and comment `/oc summarize`
```

Los nombres del proveedor, modelo y pasos siguientes dependen de la configuración elegida y pueden variar.

El workflow generado se revisará y publicará en el paso 4.

---

### 2. Administrar la aplicación OpenCode Agent

Durante la instalación se agrega `OpenCode Agent` a GitHub.

Para administrar la instalación ingresar en:

```text
settings/installations
```

Desde esta sección se puede:

- Revisar la instalación de OpenCode Agent.
- Consultar los repositorios autorizados.
- Modificar el acceso.
- Desinstalar la aplicación.

> Se mantienen estas rutas en la documentación porque permiten identificar rápidamente dónde realizar cada configuración dentro de GitHub.

---

### 3. Configurar la clave de API del proveedor

Si utilizas OpenAI, configura la siguiente clave:

```text
OPENAI_API_KEY
```

Dentro del repositorio de GitHub ingresar en:

```text
settings/secrets/actions
```

La navegación corresponde aproximadamente a:

```text
Repository
└── Settings
    └── Secrets and variables
        └── Actions
```

Desde esta sección se crea el secreto utilizado por el workflow.

> El nombre del secreto depende del proveedor seleccionado.

> `OPENAI_API_KEY` permite acceder a la API de OpenAI. Una suscripción de ChatGPT no sustituye la clave de API necesaria para ejecutar el proveedor desde GitHub Actions.

---

### 4. Publicar el workflow

Registra el workflow generado y súbelo desde `chore/opencode-github-agent`:

```bash
git status
git add .github/workflows/opencode.yml
git commit -m "Configure OpenCode GitHub agent"
git push -u origin chore/opencode-github-agent
```

Revisa el cambio mediante un pull request antes de integrarlo en `main`.

El archivo publicado puede revisarse directamente en:

[`.github/workflows/opencode.yml`](https://github.com/MLahuasi/opencode-asteroids/blob/main/.github/workflows/opencode.yml)

---

[← Anterior](04-limpieza-de-worktrees-y-ramas.md) | [Temario](../08-opencode-github.md) | [Siguiente →](06-issues-y-pull-requests.md)
