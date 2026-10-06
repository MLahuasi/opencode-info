# Preparar el proyecto y sus instrucciones

Antes de delegar cambios a OpenCode conviene preparar manualmente el proyecto, documentar los requerimientos y comprobar que las herramientas básicas funcionan.

> **Ejemplo:** los nombres de archivos, scripts y comandos de esta guía sirven para contextualizar el flujo. No son requisitos obligatorios para todos los proyectos.

## 1. Definir y configurar el proyecto

### 1. Documentar los requerimientos

El archivo principal puede ser: [README.md](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/README.md)

También se pueden agregar documentos con instrucciones específicas, por ejemplo:

[bun-instructions.md](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/bun-instructions.md)
[revision.md](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/revision.md)
architecture.md

La documentación debería definir, al menos:

- Objetivo de la aplicación.
- Tecnologías que se deben utilizar.
- Funcionalidades principales.
- Restricciones.
- Convenciones del proyecto.
- Comportamientos esperados.
- Comandos importantes.
- Requisitos de testing.
- Consideraciones de arquitectura.

Mientras más claras sean las instrucciones, menor será la cantidad de decisiones que OpenCode tendrá que asumir.

### 2. Crear manualmente la estructura inicial

Cuando sea posible, ejecuta manualmente los comandos relacionados con la creación y configuración inicial del proyecto. Por ejemplo, usando Bun:

```bash
bun init
```

Esto permite:

- Comprender la estructura inicial.
- Controlar las dependencias.
- Evitar configuraciones innecesarias.
- Saber exactamente qué archivos fueron creados.
- Reducir la cantidad de decisiones delegadas al agente.

### 3. Configurar los scripts

Los scripts principales también pueden configurarse manualmente [package.json](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/package.json):

```json
{
  "scripts": {
    "build": "bun build --compile src/index.ts --outfile weather",
    "start": "bun run src/index.ts",
    "dev": "bun run --watch src/index.ts",
    "test": "bun test"
  }
}
```

Antes de delegar tareas a OpenCode comprueba que los comandos básicos funcionen correctamente.

## 2. Crear o actualizar [README.md](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/README.md)

Una vez preparada la estructura inicial y la documentación, ejecuta OpenCode dentro del repositorio y utiliza:

```text
/init
```

OpenCode analiza el proyecto y crea o actualiza [`AGENTS.md`](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/AGENTS.md). Este archivo contiene instrucciones que OpenCode utilizará para comprender cómo debe trabajar dentro del proyecto.

Después de generarlo, revísalo cuidadosamente. Puede modificarse manualmente para:

- Agregar reglas.
- Eliminar instrucciones innecesarias.
- Definir convenciones.
- Establecer restricciones.
- Especificar comandos de validación.
- Documentar la arquitectura.
- Definir la ubicación de archivos.
- Indicar qué archivos no deben modificarse.
- Definir cómo ejecutar tests.
- Establecer criterios de aceptación.

[`AGENTS.md`](https://github.com/MLahuasi/opencode-weather-cli-app/blob/main/AGENTS.md) no debe considerarse correcto únicamente porque fue generado automáticamente. Para ampliar este tema, consulta [`/init` y `AGENTS.md`](../tui/06-init-agents-e-ignorados.md).

---

[Temario](../07_opencode-assisted-development.md) | [Siguiente →](02-planificar-y-revisar-el-plan.md)
