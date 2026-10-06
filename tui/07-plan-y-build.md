# Plan y Build

> **Ejemplo:** `Plan` y `Build` describen el flujo mostrado por la interfaz. La terminología, los permisos y los nombres pueden variar según la versión o la configuración. Consulta también [planificar](../use-mode/02-planificar-y-revisar-el-plan.md) e [implementar](../use-mode/03-implementar-revisar-y-ajustar.md).

## 1. Separar análisis e implementación

OpenCode permite separar el análisis de una tarea de su implementación. En la TUI, `Plan` y `Build` pueden aparecer como modos o agentes de trabajo, según la versión instalada.

Para cambiar la selección se utiliza:

```text
Tab
```

## 2. Trabajar en Plan

Presiona `Tab` hasta seleccionar:

```text
Plan
```

En este perfil OpenCode puede analizar una funcionalidad y proponer cómo implementarla antes de modificar archivos. También puede ser posible cambiar de modelo mientras se trabaja en `Plan`.

Para tareas complejas, esta etapa puede requerir:

- Analizar el proyecto.
- Detectar problemas.
- Evaluar alternativas.
- Tomar decisiones.
- Diseñar una estrategia.
- Crear pasos de implementación.

Una vez revisado el plan, `Build` puede utilizar un modelo menos potente si la implementación está suficientemente clara.

## 3. Pasar de Plan a Build

Selecciona:

```text
Build
```

presionando `Tab` hasta que aparezca seleccionado. Después responde a la solicitud de confirmación según corresponda, por ejemplo:

```text
si
```

El texto anterior es un ejemplo de respuesta, no una palabra reservada obligatoria.

## 4. Continuar trabajando en Build

No es necesario regresar a `Plan` para cada modificación sencilla que ya esté contemplada en el plan. Si el ajuste cambia el alcance o requiere decisiones nuevas, vuelve a `Plan` y revisa la propuesta antes de continuar.

Las transcripciones completas de este flujo se encuentran en [Ejemplos de respuestas de Plan y Build](ejemplos/respuestas-plan-build.md).

---

[← Anterior](06-init-agents-e-ignorados.md) | [Temario](../06_opencode_tui.md) | [Siguiente →](ejemplos/respuestas-plan-build.md)
