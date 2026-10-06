# Fundamentos de Spec Driven Development

Esta guía explica la metodología **Spec Driven Development (SDD)** y sus reglas básicas. El proyecto [OpenCode - Pacman](https://github.com/MLahuasi/opencode-pacman) se utiliza como laboratorio a lo largo de los ejemplos.

## Metodología Spec Driven Development

### 1. Idea principal

En **Spec Driven Development** primero se define una especificación detallada y estructurada de lo que debe hacer el software.

La implementación comienza después de entender y aprobar esa especificación.

Una regla importante al iniciar el proceso es:

> **Describe el problema, no la solución.**

El objetivo es permitir que el modelo analice el problema y proponga posibles soluciones antes de comenzar a modificar código.

### 2. Etapas del proceso

El flujo puede dividirse conceptualmente en dos etapas.

#### Fase humana: definir qué construir

El usuario y el agente trabajan sobre:

1. El problema.
2. Las decisiones técnicas.
3. El alcance.
4. Los comportamientos esperados.
5. Los criterios que permitirán comprobar el resultado.

Normalmente pueden requerirse entre **2 y 3 iteraciones** de refinamiento antes de aprobar la especificación.

#### Fase de ejecución: implementar

Después de aprobar las decisiones:

1. Se guarda la especificación.
2. OpenCode implementa sus pasos.
3. Se revisan cambios pequeños.
4. Se comprueba cada fase.
5. Finalmente se validan los criterios de aceptación.

### 3. Reglas importantes

#### Describe el problema

En la primera etapa explica qué necesitas resolver.

Evita definir prematuramente toda la solución técnica. Permite que los LLM propongan alternativas y después decide cuál utilizar.

#### Toma decisiones concretas

Durante el refinamiento evita respuestas ambiguas como:

```text
Creo que...
Tal vez sería bueno...
Podríamos intentar...
```

Conviene convertirlas en decisiones explícitas.

#### Implementa en pasos pequeños

Durante la implementación solicita pausas entre las diferentes fases del plan.

Esto permite revisar `diffs` pequeños y detectar problemas antes de que se acumulen demasiados cambios.

#### No improvises durante la implementación

Si durante la ejecución descubres que una decisión importante debe cambiar:

> Regresa al proceso de planificación.

No conviene modificar el diseño improvisadamente en medio de la implementación.

---

[Temario](../09-opencode-spec-driven-development.md) | [Siguiente →](02-la-spec-como-artefacto.md)
