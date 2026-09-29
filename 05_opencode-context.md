# Contexto en conversaciones con LLM

## 1. Qué significa que un LLM sea *stateless*

Un modelo de lenguaje (LLM) no conserva por sí solo las interacciones anteriores. Para responder de forma coherente, la aplicación le envía la información pertinente junto con cada solicitud. A ese conjunto se le llama **contexto**.

```text
Contexto = instrucciones + conversación + información adicional
```

La aplicación decide qué información incluir; no necesariamente vuelve a enviar todo el historial en cada solicitud.

## 2. Qué puede formar parte del contexto

El contexto puede combinar instrucciones y recursos con el contenido generado durante la sesión.

### Instrucciones y recursos

Según la plataforma y la configuración, puede incluir:

- System prompt.
- Herramientas (tools) y servidores MCP.
- Skills y agentes personalizados.
- Archivos de instrucciones, como `AGENTS.md` o `CLAUDE.md`.

### Contenido de la sesión

Durante una conversación pueden aportar contexto:

- Mensajes del usuario y respuestas del modelo.
- Archivos e imágenes compartidos.
- Resultados de herramientas, comandos y pruebas.
- Información recuperada durante la sesión.

El historial de la sesión suele ser el contenido que más crece y que puede resumirse o compactarse cuando deja de ser necesario en detalle.

## 3. Cómo se construye y crece el contexto

La aplicación combina la información que considera pertinente para formar el contexto de una solicitud. Una parte puede provenir de instrucciones y recursos; otra, de la conversación.

### Ejemplo de acumulación

Una conversación puede acumular mensajes y resultados anteriores:

```text
Solicitud 1: Prompt 1

Solicitud 2: Prompt 1 + Respuesta 1 + Prompt 2

Solicitud 3: Prompt 1 + Respuesta 1 + Prompt 2 + Respuesta 2 + Prompt 3
```

Cuanto más historial se incluya en una solicitud, más tokens puede consumir. La cantidad concreta depende del contexto que la aplicación decida enviar.

### Flujo de envío

```mermaid
flowchart LR
    A["Instrucciones y recursos<br/>Mensaje de sistema · Herramientas · MCP · Skills"]
    B["Conversación<br/>Mensajes · Respuestas · Archivos · Resultados de herramientas"]
    C["Nuevo mensaje"]
    D["Contexto seleccionado<br/>para la solicitud"]
    E["LLM<br/>Stateless"]
    F["Nueva respuesta"]

    A --> D
    B --> D
    C --> D
    D --> E
    E --> F
    F --> B
```

La respuesta nueva pasa a formar parte de la conversación. La aplicación puede incluirla en solicitudes posteriores junto con otra información pertinente.

## 4. Tamaño del contexto y “Smart Zone”

Una ventana de contexto grande no implica que siempre convenga llenarla. Si se envía demasiada información irrelevante, pueden aumentar el consumo de tokens y el costo, y puede resultar más difícil mantener el foco o priorizar instrucciones vigentes.

El rango de **40 % a 60 %** se presenta aquí como una referencia práctica informal, no como un límite técnico universal ni una garantía de calidad:

```text
0% ──────────── 40%–60% ──────────── 100%
               referencia informal
```

## 5. Compactar el contexto

En conversaciones largas, se puede resumir el historial antiguo y conservar la información que todavía guía el trabajo:

```text
Resumen
Decisiones importantes
Estado actual
Pendientes
Nuevo mensaje
```

La compactación resume información y puede perder detalles o matices. Revisa el resumen cuando las decisiones sean importantes y procura conservar el estado, las restricciones y los pendientes que todavía guían el trabajo.

> **El modelo no recuerda por sí solo la conversación: la aplicación le proporciona contexto. Cuando ese contexto crece, conviene conservar lo relevante y resumir lo demás.**
