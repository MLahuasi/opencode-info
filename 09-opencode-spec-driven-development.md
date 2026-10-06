# Spec Driven Development (SDD)

Esta guía explica **Spec Driven Development (SDD)**, el papel de OpenCode, las Specs, Skills, agentes y Git, y un flujo práctico para definir, implementar y verificar funcionalidades.

El proyecto [OpenCode - Pacman](https://github.com/MLahuasi/opencode-pacman) se utiliza como laboratorio. Las interacciones con OpenCode, preguntas de refinamiento y configuraciones concretas del laboratorio están separadas en documentos de ejemplo.

## Temario

### Fundamentos

1. [Fundamentos de SDD](./sdd/01-fundamentos-de-sdd.md) — metodología, etapas y reglas de trabajo.
2. [La Spec como artefacto](./sdd/02-la-spec-como-artefacto.md) — anatomía, alcance, criterios y decisiones.
3. [Specs, Skills y agentes](./sdd/03-specs-skills-y-agentes.md) — responsabilidades y relación entre los componentes.

### Preparación y control del repositorio

4. [Preparar OpenCode para SDD](./sdd/04-preparar-opencode-para-sdd.md) — instalar las Skills `spec` y `spec-impl`, y configurar `AGENTS.md`.
5. [Git y protección de la rama principal](./sdd/05-git-y-proteccion-de-main.md) — publicar, proteger `main`, crear ramas y actualizar el repositorio.

### Flujo de una Spec

6. [Crear, generar y aprobar una Spec](./sdd/06-crear-generar-y-aprobar-una-spec.md) — crear los artefactos, revisar decisiones y aprobar la especificación.
7. [Implementar, validar e integrar una Spec](./sdd/07-implementar-validar-e-integrar.md) — implementar por pasos, validar y crear el Pull Request.

### Síntesis

8. [Flujo, principios y modelo mental de SDD](./sdd/08-flujo-principios-y-modelo-mental.md) — recorrido completo y principios extraídos del laboratorio.

### Ejemplos del laboratorio

- [Crear y verificar una Skill de Pokémon](./sdd/ejemplos/skill-pokemon.md) — ejemplo de interacción con OpenCode y una Skill reutilizable.
- [Refinar una Spec para fantasmas](./sdd/ejemplos/refinamiento-spec-fantasmas.md) — preguntas y respuestas generadas durante la definición de una funcionalidad.

## Orden recomendado

Lee primero los documentos 1 a 3 para entender SDD y sus componentes. Continúa con los documentos 4 y 5 para preparar el proyecto. Después recorre los documentos 6 y 7 para crear, aprobar, implementar y cerrar una Spec. Usa los ejemplos cuando necesites ver cómo puede intervenir OpenCode durante el proceso.

## Criterio para los ejemplos

Los documentos dentro de `sdd/ejemplos/` conservan salidas, preguntas, respuestas y decisiones concretas del laboratorio. Sirven para ilustrar el proceso, pero no deben interpretarse como requisitos generales ni copiarse sin adaptar nombres, rutas, herramientas y convenciones del proyecto.

---

[← Anterior](08-opencode-github.md) | [Temario](README.md) | [Siguiente →](10-opencode-mcp.md)
