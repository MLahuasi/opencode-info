# Automatizar compilaciones y publicaciones

GitHub Actions puede automatizar la instalación de dependencias, las pruebas, la compilación, las validaciones, la generación de ejecutables y la creación de releases.

> **Ejemplo:** `ci.yml`, `release.yml`, los tags `v*`, los targets de Bun y los nombres de ejecutables son una configuración de referencia. Deben adaptarse a las plataformas y políticas del proyecto.

## 1. Separar CI y releases

Conviene separar las responsabilidades:

`ci.yml` -> pull_request y push: pruebas y validaciones
[`release.yml`](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/.github/workflows/release.yml) -> tag o publicación: build y release

La configuración normalmente se almacena en:

```text
.github/
└── workflows/
    ├── ci.yml
    └── release.yml
```

El procedimiento general para crear y publicar un workflow se explica en [Workflows personalizados de GitHub Actions](../git-github/07-github-actions-personalizados.md).

## 2. Diseñar el workflow de release

Comienza en `Plan` y solicita un workflow concreto. Por ejemplo, basado en tags con formato `v*`:

```text
Diseña un workflow de GitHub Actions para publicar una nueva versión
de la aplicación cuando se cree un tag con formato v*.

Requisitos:

- Instalar las dependencias.
- Ejecutar los tests antes del build.
- Detener el workflow si algún test falla.
- Obtener la versión desde @package.json.
- Ejecutar el comando de build definido por el proyecto.
- Generar los ejecutables soportados.
- Crear un GitHub Release asociado a la versión.
- Adjuntar los ejecutables al release.
- Revisar las variables requeridas tomando como referencia
  @.env.example.

No implementes todavía. Primero presenta el plan.
```

Después de revisar y aprobar el plan, cambia a `Build`.

## 3. Compilar ejecutables multiplataforma

El comando local:

```bash
bun build --compile src/index.ts --outfile weather
```

genera un ejecutable para el entorno utilizado. Para varias plataformas, el workflow debe utilizar una matriz de runners nativos o targets explícitos de Bun. Por ejemplo:

```bash
# Linux
bun build --compile --target=bun-linux-x64 src/index.ts --outfile weather-linux-x64

# macOS
bun build --compile --target=bun-darwin-arm64 src/index.ts --outfile weather-macos-arm64

# Windows
bun build --compile --target=bun-windows-x64 src/index.ts --outfile weather-windows-x64.exe
```

Una release multiplataforma podría contener:

```text
weather-linux-x64
weather-macos-arm64
weather-windows-x64.exe
Source code (zip)
Source code (tar.gz)
```

Los nombres, runners y arquitecturas dependerán de los sistemas operativos soportados por el proyecto.

## 4. Validar el workflow

Después de crear o modificar el workflow, publícalo mediante una rama y un Pull Request:

```bash
git status
git diff
git add .github/workflows/ci.yml .github/workflows/release.yml
git diff --cached
git commit -m "Configure GitHub Actions release workflow"
git push -u origin feature/release-workflow
```

Revisa el workflow en:

```text
GitHub
└── Actions
```

Si falla, analiza los logs antes de realizar nuevas modificaciones. Algunas causas frecuentes son:

- Tests fallidos.
- Error de compilación.
- Dependencias incorrectas.
- Variables faltantes.
- Permisos insuficientes.
- Problemas con el workflow.
- Diferencias entre sistemas operativos.
- Comandos que funcionan localmente pero no en CI.

> **Importante:** no modifiques el workflow simplemente hasta conseguir que aparezca en verde. Primero identifica la causa real del fallo.

## 5. Revisar la seguridad de los entregables

Los ejecutables son artefactos críticos para la cadena de suministro. Antes de publicar revisa:

- Código incluido.
- Dependencias.
- Variables de entorno.
- Secretos.
- Archivos empaquetados accidentalmente.
- Plataforma y arquitectura.
- Procedencia del build.
- Integridad del artefacto.
- Hashes, firmas o attestations cuando correspondan.

> **Importante:** nunca incluyas archivos `.env` reales, claves API, tokens, credenciales o secretos dentro de los ejecutables o releases.

Un ejecutable descargado desde Internet también puede generar advertencias de seguridad del sistema operativo, especialmente cuando no está firmado digitalmente.

Archivos como `.env.example` pueden incluirse como documentación si son necesarios, pero nunca deben contener secretos reales.

---

[← Anterior](05-versionar-y-colaborar.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente →](07-flujo-completo-y-checklist.md)
