# Ejemplo: refinamiento de una Spec para fantasmas

> **Ejemplo de interacción con OpenCode**
>
> Las preguntas y respuestas de este documento fueron generadas durante el laboratorio OpenCode Pacman. Sirven para ilustrar cómo refinar una solicitud; no constituyen requisitos universales para un juego de Pac-Man.

## Solicitud inicial

Para el laboratorio se implementará una funcionalidad relacionada con el comportamiento de los cuatro fantasmas de Pac-Man.

En modo `Plan`, ejecutar `/spec` seguido del requerimiento.

Aunque `/spec` no aparezca visualmente entre los comandos disponibles, puede invocarse directamente.

```text
/spec En PacMan existen 4 fantasmas, cada uno tiene un comportamiento propio (diferente), uno de ellos debe perseguir agresivamente a PacMan
```

El objetivo inicial describe principalmente el comportamiento deseado, no todos los detalles de implementación.

## Refinar la especificación

La Skill comenzará a realizar preguntas para convertir el requerimiento inicial en decisiones concretas.

### Comportamiento de los fantasmas

```text
¿Qué nivel de fidelidad deben tener los cuatro comportamientos?

4. Type your own answer:
(a) hunter - persigue directamente;
(b) ambusher - apunta a N celdas por delante de PacMan;
(c) patrol - alterna entre esquinas;
(d) random - aleatorio.
```

### Nombres y colores

```text
¿Los fantasmas deben usar nombres y colores clásicos?

1. Clásico (Recommended)

Blinky rojo, Pinky rosa, Inky cian y Clyde naranja.
```

### Fantasma agresivo

```text
¿Cómo debe perseguir el fantasma agresivo a PacMan?

1. Ruta más corta (Recommended)

En cada intersección elige el primer paso de una ruta válida.
```

### Persecución y dispersión

```text
¿Esta spec debe incluir alternancia entre persecución y dispersión como en el arcade?

1. No incluir (Recommended)

Cada fantasma conserva su conducta propia durante toda la partida.
```

### Salida de la guarida

```text
¿Cómo deben empezar a moverse los cuatro fantasmas desde la guarida?

4. Type your own answer

Salen escalonadamente uno a uno cada 4 segundos.
```

## Refinamiento adicional

Después de las primeras respuestas pueden aparecer nuevas decisiones.

### Relación entre nombre y comportamiento

```text
¿Qué nombre clásico corresponde a cada comportamiento?

1. Blinky/Pinky/Inky/Clyde (Recommended)

Blinky hunter,
Pinky ambusher,
Inky patrol,
Clyde random.
```

### Distancia de emboscada

```text
¿A cuántas celdas por delante debe apuntar el fantasma ambusher?

1. 4 celdas (Recommended)

Referencia clásica y diferencia claramente la emboscada.
```

### Comportamiento de patrulla

```text
¿Cómo alterna el fantasma patrol entre esquinas?

1. Al llegar al objetivo (Recommended)

Alterna entre esquina superior derecha e inferior izquierda al alcanzar cada destino.
```

### Escalonamiento

```text
¿El escalonamiento debe ser Blinky inmediato y los demás a los 4, 8 y 12 segundos,
reiniciándose también después de perder una vida?

1. Sí, siempre (Recommended)

Se aplica al inicio y tras cada reposición de posiciones.
```

### Elementos fuera del alcance

```text
¿Qué debe quedar expresamente fuera de esta spec? (select all that apply)

5. [x] Type your own answer

Todas las anteriores.
```

## Decisiones finales

### Posición inicial

```text
¿Dónde deben empezar los 4 fantasmas?

1. Todos dentro, separados (Recommended)
```

### Orden de salida

```text
¿Confirmamos el orden Blinky 0 s, Pinky 4 s, Inky 8 s y Clyde 12 s?

1. Sí (Recommended)
```

### Medición del tiempo

```text
¿Cómo se miden los intervalos de 4 segundos?

1. Tiempo activo real (Recommended)
```

### Objetivos inválidos

```text
Si el objetivo de ambusher o patrol cae en un muro o no tiene ruta,
¿qué debe hacer?

1. Celda válida más cercana (Recommended)
```

Estas iteraciones convierten una solicitud relativamente ambigua en un conjunto de decisiones concretas que posteriormente pueden implementarse y verificarse.

---

[← Volver a crear una Spec](../06-crear-generar-y-aprobar-una-spec.md) | [Temario](../../09-opencode-spec-driven-development.md)
