# Contexto y compactación

> **Ejemplo:** los porcentajes de este documento son referencias informales, no límites técnicos ni reglas universales de OpenCode.

## 1. Observar el contexto

OpenCode muestra un porcentaje `%` que permite conocer cuánto contexto de la sesión se está utilizando. El espacio disponible depende del modelo, de la configuración y de la solicitud.

Cuando el porcentaje se acerque a una referencia alta, considera:

- Compactar la conversación.
- Iniciar una nueva sesión.
- Separar el trabajo en sesiones más pequeñas.
- Evitar cargar archivos o directorios innecesarios.

Un contexto demasiado grande puede dificultar que el modelo identifique qué información es importante. Para una explicación general del contexto, consulta [Contexto en conversaciones con LLM](../05_opencode-context.md).

## 2. Compactar una sesión

El comando siguiente solicita un resumen de la conversación:

```text
/compact
```

El modelo decide qué información conservar, por lo que puede omitir datos que después resulten importantes. Utilízalo con precaución.

Antes de ejecutar `/compact`, documenta la información crítica, como:

- Decisiones.
- Configuraciones.
- Arquitectura.
- Comandos importantes.
- Restricciones.
- Reglas del proyecto.

La compactación permite continuar trabajando con un resumen en lugar de mantener todo el historial original dentro del contexto.

---

[← Anterior](02-sesiones-y-panel-lateral.md) | [Temario](../06_opencode_tui.md) | [Siguiente →](04-modo-shell.md)
