# Planificar y revisar el plan

> **Ejemplo:** las solicitudes y decisiones mostradas aquí ilustran una forma posible de trabajar con `OpenCode`. Adáptalas a los objetivos y restricciones de tu proyecto.

## 1. Diseñar la implementación con `Plan`

Para desarrollar una funcionalidad es recomendable comenzar trabajando en el agente primario `Plan`. Puedes cambiar de agente con `Tab`, salvo que el atajo `switch_agent` haya sido personalizado.

`Plan` está diseñado para analizar la solicitud y proponer una estrategia antes de modificar el proyecto. Con la configuración predeterminada, las ediciones y los comandos de shell requieren confirmación; los permisos efectivos dependen de la configuración del proyecto.

Esta fase es especialmente importante cuando existen:

- Decisiones arquitectónicas.
- Varias alternativas de implementación.
- Nuevas dependencias.
- Cambios que afectan varios archivos.
- Migraciones.
- Nuevas funcionalidades.
- Cambios importantes en testing o infraestructura.

Conviene utilizar un modelo con buena capacidad de razonamiento durante esta etapa.

Ejemplo:

```text
Vas a desarrollar una aplicación que obtiene información
del clima de una ciudad.

Revisa los requerimientos definidos en @README.md y diseña
un plan de implementación.
```

## 2. Revisar el plan

Durante la planificación OpenCode puede solicitar información adicional o presentar diferentes alternativas sobre:

- Arquitectura.
- Dependencias.
- Diseño de la interfaz.
- Manejo de errores.
- Persistencia.
- Testing.
- Organización del código.
- Integraciones externas.
- Estrategias de configuración.

Cuando se presenten varias opciones se puede:

1. Seleccionar una alternativa.
2. Ingresar una respuesta personalizada.
3. Solicitar otra alternativa.
4. Pedir cambios al plan.

> **Importante:** si el plan no cumple con lo esperado, solicita ajustes antes de comenzar la implementación.

No pases a `Build` hasta comprender y aprobar el plan propuesto. Para ver un ejemplo aplicado a una rama de funcionalidad, consulta [Desarrollo local con ramas](../git-github/01-flujo-local-con-ramas.md).

---

[← Anterior](01-preparar-proyecto-e-instrucciones.md) | [Temario](../07_opencode-assisted-development.md) | [Siguiente →](03-implementar-revisar-y-ajustar.md)
