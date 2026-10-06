# Ejemplos de respuestas de Plan y Build

> **Ejemplos de respuestas de OpenCode:** las transcripciones de este documento sirven para dar contexto sobre una posible interacción. No son salidas garantizadas, sintaxis que debas copiar ni reglas universales. Los archivos, líneas, tiempos y decisiones dependen del proyecto y de la versión instalada.

## 1. Solicitud en Plan

Solicitud de ejemplo:

```text
Instala Tailwind CSS en este proyecto
```

OpenCode podría preguntar:

```text
¿Qué método de instalación de Tailwind CSS prefieres?

1. CDN
   Agrega una etiqueta <script> al HTML. No requiere npm ni build.
   Puede ser adecuado para un prototipo o una demo.

2. CLI + npm
   Instala Tailwind mediante npm y genera CSS con el CLI.
   Requiere un paso de build.

3. Vite + PostCSS
   Configura Tailwind con Vite como bundler. Requiere package.json,
   configuración de PostCSS y un build.

4. Type your own answer

↑↓ select   enter submit   esc dismiss
```

## 2. Respuesta de planificación

Si se selecciona la primera opción, una respuesta posible sería:

```text
Plan: Incorporar Tailwind CSS mediante CDN

El método CDN agrega un script al <head> de index.html. No instala una
dependencia npm ni configura un paso de compilación.

Pasos

1. Agregar el script CDN en index.html.
2. Migrar el CSS inline existente a clases de Tailwind.
3. Corregir el tag de cierre incompleto si existe.

¿Procedo con la implementación?
```

La elección de CDN es solo un ejemplo. El método correcto depende de los objetivos del proyecto, sus restricciones y la versión de Tailwind.

## 3. Respuesta de implementación en Build

Después de seleccionar `Build` y confirmar, OpenCode podría mostrar una transcripción similar a la siguiente:

```text
← Edit index.html

7 -    <style>
8 -      body { background-color: #FF0000; }
9 -    </style>
7 +    <script src="https://cdn.tailwindcss.com"></script>
9 +    <body class="bg-red-600">

+ Thought: 130ms
→ Read index.html

Listo. El ejemplo incorpora Tailwind mediante CDN en index.html.
Abre el archivo en el navegador para verificar el resultado.
```

Este bloque es una transcripción ilustrativa de la actividad de la TUI, no código que debas pegar en un archivo.

## 4. Ajuste posterior en Build

Solicitud de ejemplo:

```text
Cambia el color de fondo a morado pastel usando Tailwind.
```

Una respuesta posible sería:

```text
+ Thought: 1.4s
← Edit index.html

9 -  <body class="bg-red-600">
9 +  <body class="bg-purple-200">

Fondo cambiado a bg-purple-200 en index.html.
```

Si el ajuste cambia el alcance o requiere decisiones nuevas, vuelve a `Plan` y revisa la propuesta antes de continuar.

---

[← Anterior](../07-plan-y-build.md) | [Temario](../../06_opencode_tui.md) | [Siguiente →](../08-referencia-de-atajos.md)
