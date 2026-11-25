# Fundamentos de Grill-me

Grill-me es una Skill para convertir una idea poco definida en un conjunto de decisiones que se puedan defender antes de comprometerse con un plan o una implementación. Lo hace mediante una conversación de preguntas organizadas por rondas, explorando las decisiones y supuestos que todavía no están resueltos.

> Grill-me no es una metodología de desarrollo completa ni sustituye la decisión del equipo. Es una técnica de alineación y exploración que puede utilizarse antes de SDD, de una implementación directa o de cualquier otra metodología.

Fuentes consultadas:

- [The `/grill-me` Skill](https://www.aihero.dev/skills-grill-me), de AI Hero.
- [grill-me en skills.sh](https://www.skills.sh/mattpocock/skills/grill-me), de Matt Pocock.

## 1. ¿Qué problema resuelve?

Una idea inicial suele contener decisiones implícitas: alcance, usuarios, restricciones, casos límite, prioridades y criterios para saber si la solución funciona. Implementar directamente con esos aspectos sin aclarar puede producir planes aparentemente completos, pero basados en supuestos que nadie ha validado.

Grill-me ayuda a descubrir esos supuestos mediante preguntas sistemáticas. La sesión termina cuando ya no quedan ramas relevantes del problema por explorar o cuando se identifica que hace falta construir un prototipo para responder una pregunta que no puede resolverse conversando.

## 2. Características principales

### Conversación por rondas

Las preguntas se organizan en rondas. Cada ronda explora la frontera actual: las preguntas cuyos requisitos previos ya fueron respondidos. Por eso, la Skill evita preguntar detalles que dependen de decisiones todavía desconocidas.

### Interrogación exhaustiva

El objetivo no es confirmar rápidamente la primera idea. Grill-me debe ponerla a prueba, recorrer las alternativas relevantes y revelar decisiones que estaban ocultas. Una buena sesión puede llevar a una solución distinta de la propuesta inicial.

### Sin estado y sin archivos

La Skill `grill-me` es **stateless**: no escribe archivos ni deja artefactos en el repositorio. El resultado es el entendimiento construido durante la conversación. Si se necesita registrar decisiones, el usuario puede continuar con otra herramienta o metodología después de la sesión.

### Aplicable fuera del código

No requiere un repositorio y el tema no tiene que ser una funcionalidad de software. Puede utilizarse para explorar una decisión de negocio, una dirección de producto, un texto o cualquier idea que necesite mayor claridad.

## 3. Cuándo utilizar Grill-me

Utiliza Grill-me cuando:

- tienes una idea importante, pero todavía es vaga;
- necesitas descubrir alcance, restricciones o criterios de éxito;
- quieres revisar una decisión de arquitectura o diseño antes de implementarla;
- varias alternativas parecen válidas y necesitas comparar sus consecuencias;
- quieres identificar riesgos y casos límite antes de escribir un plan;
- el equipo necesita construir una comprensión compartida.

No es necesario esperar a tener un plan completo. Precisamente, la sesión sirve para transformar una intuición en decisiones explícitas.

## 4. Cuándo no utilizarlo

No necesitas Grill-me si la tarea ya está especificada con precisión y solo falta ejecutarla. Tampoco conviene prolongar la conversación cuando la respuesta depende de algo que solo puede evaluarse observando un resultado.

Son ejemplos de preguntas que pueden ser **no grillables**:

- ¿Es mejor un formulario largo o tres pantallas?
- ¿Cómo debería sentirse una interacción?
- ¿Qué variante visual entenderán mejor los usuarios?

Ante este tipo de incertidumbre, crea un prototipo desechable, observa el resultado y vuelve a decidir con evidencia. Preguntar indefinidamente sobre algo que todavía no se puede ver suele aumentar la incertidumbre en lugar de reducirla.

## 5. Cómo iniciar una sesión

Para instalar las Skills utiliza el procedimiento publicado en [skills.sh](https://www.skills.sh/mattpocock/skills/grill-me). Es necesario instalar `grill-me` y `grilling`:

```bash
npx skills add https://github.com/mattpocock/skills --skill grill-me --skill grilling
```

Después de instalarlas, inicia la sesión de preguntas desde OpenCode utilizando:

```text
@grilling
```

`npx skills add` instala ambas Skills y `@grilling` inicia la entrevista sistemática para obtener información y decisiones.

Durante la sesión:

1. Describe la idea inicial sin intentar resolver todos sus detalles.
2. Responde con precisión y señala los supuestos que no conoces.
3. Corrige al agente si la pregunta es ambigua o si el alcance se está desviando.
4. Expresa desacuerdo cuando una alternativa no te convenza.
5. Divide el problema si la conversación se vuelve demasiado grande.
6. Continúa con la misma conversación cuando quieras transformar las decisiones en una Spec o en un plan.

## 6. Responsabilidad de la persona

Grill-me formula preguntas, pero la persona mantiene la responsabilidad del alcance y de las decisiones. Responder automáticamente “de acuerdo” a todas las preguntas puede producir una falsa sensación de avance: el agente habrá construido el plan, pero el equipo no habrá elegido conscientemente sus decisiones.

Una sesión útil permite:

- cuestionar una premisa;
- responder “no lo sé” cuando aún no existe información suficiente;
- indicar que una pregunta es demasiado detallada;
- detener una línea de exploración que no aporta valor;
- defender las decisiones tomadas al final de la sesión.

## 7. Resultado y continuidad con SDD

La sesión de `@grilling` sirve para obtener información, descubrir requisitos y tomar decisiones. No debe utilizarse como sustituto del flujo de ejecución: durante el grilling no se deben implementar cambios.

Cuando la idea sea una funcionalidad de software, se recomienda continuar con [Spec Driven Development](../09-opencode-spec-driven-development.md):

1. Ejecuta `@grilling` y responde las preguntas.
2. Solicita una Spec con `/spec` usando la información obtenida.
3. Revisa y aprueba el plan y la Spec.
4. Cambia al modo build para crear la Spec y ejecutar la implementación acordada.

Ejemplo de solicitud para generar la Spec:

```text
/spec con la información obtenida mediante el grilling anterior genera una nueva Spec
```

En modo build puedes solicitar la ejecución con:

```text
crea la Spec en base al plan. Puedes colocar los prompts, respuestas en formatos text,
bash y PowerShell dentro del Markdown.
```

La conversación conserva el contexto obtenido durante `@grilling`; SDD se encarga de convertirlo en un artefacto revisable y de guiar la implementación. Grill-me no genera por sí mismo una Spec ni modifica el repositorio.

Una instrucción más explícita para la etapa de especificación sería:

```text
Convierte las decisiones acordadas durante esta conversación en una Spec con alcance,
criterios de aceptación, restricciones y decisiones descartadas.
```

## 8. Ejemplo: ajustes de la cuenta del padre de familia

Este flujo se utilizó para definir la página `/family-feed/account`, accesible desde el menú **Mi Cuenta** del Family Feed. Primero se ejecutó el grilling sin implementar cambios:

```text
@grilling Se debe crear la pagina que administra los ajustes de la cuenta del padre de familia,
se va a invocar desde el menú "Mi Cuenta" del family-feed. No implementes los cambios aún.
```

### Decisiones obtenidas

| Área | Decisión |
| --- | --- |
| Alcance | Crear `/family-feed/account` como página completa. |
| Perfil | Permitir editar nombre y teléfono; el correo permanece no editable. |
| Secciones | Mantener perfil, hijos, notificaciones, contraseña, soporte y logout. |
| Hijos | Mostrar los hijos vinculados en modo lectura reutilizando la lectura existente. |
| Notificaciones | Mostrar controles visibles, deshabilitados mientras no exista backend. |
| Contraseña | Implementar el flujo real con solo nueva contraseña y confirmación; no solicitar la contraseña actual. |
| Diseño | Seguir la referencia visual y los componentes de Family Feed. |
| Teléfono | Añadir `phone text null`; aceptar formatos internacionales y conservar el prefijo `+`. |
| Perfil | Guardar nombre y teléfono con una única Server Action, validación, actualización del usuario autenticado y revalidación. |
| Avatar | Conservar únicamente la inicial, sin carga de imágenes. |
| Guardado | Usar un botón explícito con estados de carga, éxito y error. |
| Seguridad | Derivar la identidad exclusivamente de la sesión, sin confiar en `personId` del cliente. |
| Migración | Crear una migración versionada, validarla localmente y dejarla preparada para el entorno remoto de test. |
| Acceso remoto | No aplicar cambios remotos directamente desde la implementación; usar el flujo remoto autorizado. |

La política de contraseña exige un mínimo de ocho caracteres y coincidencia entre los dos campos. La sesión debe mantenerse después del cambio. Soporte queda deshabilitado hasta definir su destino y logout se reutiliza.

El resultado del grilling se pasó después a `/spec` y, tras revisar el plan, al modo build. Así se separan el descubrimiento de requisitos, la especificación y la ejecución.

## 9. Relación con otras Skills de exploración

| Skill | Uso | Estado |
| --- | --- | --- |
| `grill-me` | Explorar cualquier idea sin leer el repositorio ni escribir archivos. | Sin estado. |
| `grill-with-docs` | Realizar una entrevista alineada con el código y registrar lo aprendido en documentación y ADRs. | Con estado. |
| `wayfinder` | Dividir un esfuerzo demasiado grande en un mapa y sesiones de exploración más pequeñas. | Para iniciativas amplias. |

La elección depende del contexto. Usa `grill-me` para una idea independiente; usa una variante con documentación cuando el código existente sea parte esencial de las respuestas.

## 10. Señales de que la sesión funciona

- Las preguntas llegan en rondas y las posteriores dependen de las respuestas anteriores.
- Aparecen decisiones que no se habían formulado explícitamente.
- Se descubre una alternativa o un riesgo inesperado.
- La persona puede explicar y defender las decisiones al finalizar.
- La conversación avanza hacia un alcance concreto, en lugar de acumular detalles sin prioridad.

Si la sesión se prolonga demasiado, reduce el alcance y trabaja cada parte por separado. Como referencia, una sesión puede contener muchas preguntas repartidas en pocas rondas; conviene evaluar el progreso por rondas y por decisiones resueltas, no solo por el número de preguntas.

## 11. Recomendaciones

- Instala `grill-me` y `grilling` juntos con el comando documentado.
- Inicia la sesión en una conversación nueva usando `@grilling`.
- No implementes cambios mientras se están descubriendo requisitos.
- Continúa con SDD para generar la Spec, revisarla y manejar la ejecución.
- Desactiva el modo de planificación si hace que el agente intente producir un plan antes de terminar las preguntas.
- Utiliza el modelo más capaz disponible cuando el análisis dependa de anticipar fallos del sistema.
- No confundas una conversación larga con una decisión de calidad.
- Prototipa las preguntas que no puedan responderse de forma fiable conversando.
- Conserva la misma conversación al pasar a la especificación para no perder el contexto construido.
- Revisa las decisiones antes de convertirlas en cambios de código.

---

[← Metodologías](../temarios/04-metodologias-de-desarrollo-de-software.md) | [Índice principal](../README.md)
