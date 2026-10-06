# Crear, generar y aprobar una Spec

Una vez configurado el proyecto puede comenzar el flujo real de Spec Driven Development.

El ejemplo completo de preguntas y respuestas utilizado para definir la funcionalidad se encuentra en [Refinamiento de una Spec para fantasmas](./ejemplos/refinamiento-spec-fantasmas.md).

## 1. Crear una Spec

Durante la fase de planificación, describe el problema y refina las decisiones con el agente. Evita implementar hasta que el alcance, las decisiones y los criterios de aceptación estén aprobados.

## 2. Generar el archivo Spec

Una vez finalizado y aprobado el `Plan`, cambiar a modo `Build` y solicitar:

```text
Crea el spec
```

La operación debe generar los artefactos de especificación.

### 2.1 Configuración de Specs

La primera vez puede generarse:

[specs/.spec-config.yml](https://github.com/MLahuasi/opencode-pacman/blob/main/specs/.spec-config.yml)

Ejemplo:

```yml
AutoCreateBranch: true
```

Esta configuración controla, entre otros aspectos, si durante la implementación de una `spec` debe crearse automáticamente una rama.

### 2.2 Archivo de especificación

Para este laboratorio se genera:

[01-four-ghost-behaviors.md](https://github.com/MLahuasi/opencode-pacman/blob/main/specs/01-four-ghost-behaviors.md)

El nombre y contenido cambiarán dependiendo de la funcionalidad.

La estructura general será similar a:

```text
specs/
├── .spec-config.yml
└── 01-four-ghost-behaviors.md
```

> **IMPORTANTE:** durante esta fase solo debe crearse la `spec`. No deben implementarse todavía cambios en el código fuente.

Esta separación es fundamental:

```text
Diseñar → Revisar → Aprobar → Implementar
```

y no:

```text
Diseñar + implementar simultáneamente
```

## 3. Aprobar la Spec

Antes de comenzar la implementación se debe revisar manualmente:

[01-four-ghost-behaviors.md](https://github.com/MLahuasi/opencode-pacman/blob/main/specs/01-four-ghost-behaviors.md)

Verificar principalmente:

- Objetivo.
- Alcance.
- Elementos fuera del alcance.
- Decisiones.
- Plan de implementación.
- Criterios de aceptación.

Si la especificación cumple con lo solicitado, cambiar su estado a:

```md
> **Status:** Approved
```

La aprobación representa la transición entre la fase de diseño y la fase de ejecución.

---

[← Anterior](05-git-y-proteccion-de-main.md) | [Temario](../09-opencode-spec-driven-development.md) | [Siguiente →](07-implementar-validar-e-integrar.md)
