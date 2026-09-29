# OpenCode: TUI (Terminal User Interface)

Esta guía cubre el uso de OpenCode desde la terminal: navegación, sesiones, contexto, herramientas y flujo de trabajo `Plan`/`Build`. Los atajos predeterminados pueden cambiar según la versión o la configuración; consulta la [documentación de la TUI](https://opencode.ai/v2/docs/cli/tui/) y la [referencia de atajos](https://opencode.ai/v2/docs/cli/keybinds/) si alguno no coincide.

## 1. Iniciar OpenCode

Una vez instalado `OpenCode`, puede utilizarse desde cualquier terminal, por ejemplo:

- PowerShell
- Warp
- CMD
- Otras terminales compatibles

Primero se debe ingresar al directorio del proyecto:

```bash
cd <directorio-del-proyecto>
```

Luego ejecutar:

```bash
opencode
```

OpenCode se iniciará utilizando el directorio actual como proyecto de trabajo.

---

## 2. Comandos básicos de la TUI

### Mostrar comandos

El atajo:

```text
Ctrl + P
```

muestra los comandos disponibles de OpenCode.

---

### Borrar el prompt

Para borrar desde el cursor hasta el inicio de la línea:

```text
Ctrl + U
```

Para borrar la palabra anterior:

```text
Ctrl + W
```

---

### Referenciar archivos con `@`

El carácter:

```text
@
```

permite buscar y referenciar archivos dentro del prompt.

Por ejemplo:

```text
@index.html
```

Esto permite indicarle explícitamente a OpenCode qué archivo debe considerar.

También puede utilizarse para cargar documentación específica cuando sea necesaria:

```text
@naming_rules.md
@architecture_rules.md
```

---

## 3. Comandos `/`

Al escribir:

```text
/
```

OpenCode muestra los comandos disponibles.

Algunos comandos frecuentes son:

```text
/compact
/connect
/fork
/init
/models
/new
/redo
/themes
/undo
```

La lista disponible puede variar según la versión y la configuración. Usa `/` para consultar los comandos de tu sesión.

---

## 4. Sesiones

### Crear una nueva sesión

El comando:

```text
/new
```

crea una nueva sesión.

---

### Continuar la sesión anterior

Si se salió de OpenCode accidentalmente o por cualquier otro motivo, se puede recuperar la sesión anterior iniciando OpenCode con:

```bash
opencode -c
```

---

### Mostrar sesiones anteriores

El atajo:

```text
Ctrl + X
L
```

muestra las sesiones ejecutadas anteriormente.

Desde esta pantalla se puede seleccionar la sesión con la que se quiere continuar trabajando.

---

### Renombrar la sesión actual

El comando:

```text
Ctrl + R
```

permite cambiar el nombre de la sesión actual.

Asignar nombres descriptivos a las sesiones facilita encontrarlas posteriormente.

---

## 5. Información de la sesión

### Barra lateral

El atajo:

```text
Ctrl + X
B
```

abre la barra lateral de OpenCode.

Esta pantalla muestra información como:

- Consumo de tokens.
- Costos.
- Contexto utilizado por la sesión.

Para cerrar nuevamente la barra lateral:

```text
Ctrl + X
B
```

---

## 6. Contexto de la conversación

OpenCode muestra un porcentaje `%` que permite conocer cuánto contexto de la sesión se está utilizando.

Como orientación informal, estas notas sugieren mantener el uso aproximadamente por debajo de:

```text
50% - 60%
```

No es un límite técnico universal: el espacio disponible depende del modelo y de la solicitud. Cuando el porcentaje se acerque a esa referencia, se puede considerar:

- Compactar la conversación.
- Iniciar una nueva sesión.
- Separar el trabajo en sesiones más pequeñas.
- Evitar cargar archivos o directorios innecesarios.

Un contexto demasiado grande puede hacer que el modelo tenga más dificultad para identificar qué información es realmente importante.

---

## 7. Compactar una sesión

El comando:

```text
/compact
```

permite reducir el contexto actual.

El LLM genera un resumen de la conversación conservando la información que considera más importante.

Sin embargo:

> El modelo decide qué información conservar.

Por este motivo, `/compact` puede eliminar información que posteriormente resulte importante.

Debe utilizarse con precaución.

Como señal aproximada —no como umbral oficial— se puede considerar compactar cuando el contexto se acerque al:

```text
50%
```

Antes de ejecutar `/compact`, es recomendable que información crítica como:

- Decisiones.
- Configuraciones.
- Arquitectura.
- Comandos importantes.
- Restricciones.
- Reglas del proyecto.

esté correctamente documentada.

La compactación permite continuar trabajando utilizando un resumen de la sesión en lugar de mantener todo el historial original dentro del contexto.

---

## 8. Modo Shell

Escribe `!` al inicio de un prompt vacío para entrar al modo `shell` y ejecutar un comando:

```text
!node --version
```

La salida del comando se incorpora a la conversación y puede usarse como contexto. Por ejemplo, después puedes preguntar qué versión de Node.js se está ejecutando.

Para salir del modo `shell` sin ejecutar el comando, presiona `Esc`.

---

## 9. Configurar Git para Undo y Redo

Para que `/undo` y `/redo` puedan restaurar también los cambios realizados sobre los archivos, el proyecto debe estar asociado a Git.

Es recomendable realizar esta configuración directamente desde la terminal normal para no utilizar tokens innecesariamente.

### Inicializar Git

Desde el directorio del proyecto:

```bash
git init
```

---

### Agregar los archivos

Antes de agregar archivos, revisa qué se incluirá y confirma que los secretos estén excluidos en `.gitignore`:

```bash
git status
git add .
```

---

### Crear el primer commit

```bash
git commit -m "first commit"
```

El proyecto tendrá ahora un estado inicial que Git puede utilizar como referencia.

---

### Reiniciar OpenCode

Después de configurar Git, inicia OpenCode desde el directorio del proyecto. Si ya estaba abierto y no reconoce el repositorio, reinícialo:

```bash
opencode
```

Esto permite que OpenCode detecte la configuración Git del proyecto.

Ahora los comandos:

```text
/undo
/redo
```

y sus respectivos atajos pueden utilizar Git para revertir o restaurar también cambios realizados sobre los archivos.

---

## 10. Undo y Redo

`/undo` revierte el último mensaje del usuario y las respuestas posteriores. Para que también pueda restaurar los cambios de archivos, el proyecto debe ser un repositorio Git.

### Undo

El comando:

```text
/undo
```

retorna al estado anterior del chat.

También se puede utilizar:

```text
Ctrl + X
U
```

Si el proyecto aún no está bajo Git, sigue los pasos de la sección anterior antes de depender de Undo/Redo para revertir cambios en archivos.

---

### Redo

El comando:

```text
/redo
```

restaura los cambios revertidos por `/undo`.

También se puede utilizar:

```text
Ctrl + X
R
```

La restauración de cambios en archivos también depende del historial Git del proyecto.

---

## 11. `/init` y `AGENTS.md`

El comando:

```text
/init
```

permite generar el archivo:

```text
AGENTS.md
```

Este archivo contiene instrucciones y contexto que OpenCode utilizará para trabajar con el proyecto.

---

### Crear primero `README.md`

Antes de ejecutar `/init` es **muy importante crear un `README.md` explicando claramente qué se quiere realizar en el proyecto**.

El `README.md` proporciona información que OpenCode puede analizar para comprender:

- El propósito del proyecto.
- Qué se quiere construir.
- Las tecnologías utilizadas.
- La estructura general.
- Las características relevantes del repositorio.

Por ejemplo:

```md
# Mi Primera Pagina Web

Esta es una página web en la cual vamos a usarla para explorar y aprender un poco sobre OpenCode.

## Tecnologías Usadas

- HTML
- CSS
- Tailwind CSS
- JavaScript

## Contacto

- Email: [jmlahuasiq@gmail.com](mailto:jmlahuasiq@gmail.com)
```

Después de crear el `README.md`, ejecutar desde OpenCode:

```text
/init
```

OpenCode analiza el proyecto y puede generar un `AGENTS.md` similar a:

```md
# AGENTS.md

- This is a minimal static HTML demo; `index.html` is the only application entrypoint.
- There is no package manager, build step, dev server, lint, typecheck, or test suite. Verify changes by opening `index.html` in a browser or serving the repository with any local static file server.
- `index.html` currently uses inline CSS only. Although `README.md` lists Tailwind CSS and JavaScript, neither is wired into the page.
- Repository prose is written in Spanish; preserve that language when updating `README.md`.
```

---

### Mantener `AGENTS.md` pequeño

`AGENTS.md` no debería ser demasiado extenso.

Su contenido forma parte del contexto que OpenCode proporciona al LLM, por lo que agregar demasiada información aumenta permanentemente el contexto utilizado.

Es preferible colocar únicamente reglas globales realmente importantes.

Por ejemplo:

- Arquitectura principal.
- Convenciones generales.
- Restricciones importantes.
- Tecnologías utilizadas.
- Reglas que deben aplicarse siempre.

---

### Separar reglas específicas

Cuando existe documentación más extensa, es mejor mantenerla en archivos separados.

Por ejemplo:

```text
naming_rules.md
architecture_rules.md
testing_rules.md
```

Cuando sea necesario utilizar una de estas reglas se puede referenciar explícitamente:

```text
@naming_rules.md
```

o:

```text
@architecture_rules.md
```

De esta manera, el agente puede cargar información específica cuando sea necesaria sin mantenerla permanentemente dentro de `AGENTS.md`.

Esto ayuda a controlar el crecimiento del contexto.

---

## 12. Patrones a ignorar

OpenCode respeta las declaraciones existentes en:

```text
.gitignore
```

Sin embargo, existen situaciones donde un archivo o directorio debe formar parte del repositorio Git pero no debería ser considerado por OpenCode.

Para estos casos se puede utilizar:

```text
.ignore
```

---

### Ejemplo

Supongamos un monorepo donde existe:

```text
auth/
frontend/
payments/
shared/
```

Si actualmente se está trabajando en otra parte del proyecto y `auth` no es necesario para OpenCode, se puede agregar al archivo `.ignore`:

```text
auth/**
```

El directorio continúa formando parte del repositorio Git porque no está excluido en `.gitignore`.

Sin embargo, OpenCode no necesita considerarlo durante sus búsquedas o exploraciones.

Esto ayuda a evitar:

- Búsquedas innecesarias.
- Archivos irrelevantes.
- Ruido dentro del proyecto.
- Aumento innecesario del contexto.

---

### Diferencia práctica

`.gitignore` indica a Git qué archivos no rastreados debe ignorar; no deja de rastrear archivos que ya fueron agregados al repositorio.

`.ignore` permite afinar las búsquedas de archivos de OpenCode. Puede excluir rutas con patrones normales, como `auth/**`, o volver a incluir rutas ignoradas por `.gitignore` con patrones negados, como `!auth/`.

Esto resulta especialmente útil en:

- Monorepos.
- Repositorios grandes.
- Proyectos con varios servicios.
- Directorios generados.
- Componentes que no forman parte de la tarea actual.

---

## 13. Plan Mode y Build Mode

OpenCode permite separar el análisis de una tarea de su implementación.

Los modos principales son:

```text
Plan
Build
```

Para cambiar entre ellos se utiliza:

```text
Tab
```

---

## 14. Plan Mode

Presionar:

```text
Tab
```

hasta seleccionar:

```text
Plan
```

En este modo OpenCode puede analizar una funcionalidad y proponer cómo implementarla antes de modificar los archivos.

También es posible cambiar de modelo mientras se trabaja en modo `Plan`.

Para tareas complejas es recomendable utilizar en `Plan` un modelo más fuerte, ya que esta etapa puede requerir:

- Analizar el proyecto.
- Detectar problemas.
- Evaluar alternativas.
- Tomar decisiones.
- Diseñar una estrategia.
- Crear los pasos de implementación.

Una vez definido correctamente el plan, en `Build` se puede utilizar un modelo menos poderoso si la implementación es suficientemente clara.

---

## 15. Ejemplo de Plan Mode

Solicitud:

```text
Instala Tailwind CSS en este proyecto
```

OpenCode puede analizar el proyecto y preguntar:

```text
¿Qué método de instalación de Tailwind CSS prefieres?

1. CDN (Recomendado)
   Agrega una etiqueta <script> al HTML. No requiere npm ni build.
   Ideal para prototipos y demos.

2. CLI + npm
   Instala Tailwind vía npm y genera un CSS estático con el CLI.
   Requiere un paso de build manual.

3. Vite + PostCSS
   Configura Tailwind con Vite como bundler. Requiere package.json,
   postcss.config y build.

4. Type your own answer

↑↓ select   enter submit   esc dismiss
```

Si se selecciona:

```text
1. CDN
```

el modo `Plan` puede responder:

```text
Plan: Instalar Tailwind CSS vía CDN

El método CDN es el más simple: solo necesitas agregar un <script> al
<head> de index.html.

Pasos

1. Agregar script de Tailwind CSS CDN en index.html — insertar la
   etiqueta <script src="https://cdn.tailwindcss.com"></script>
   dentro de <head>, antes de la etiqueta <style>.

2. Migrar el CSS inline existente (el background-color: #FF0000) a
   clases de Tailwind: bg-red-600.

3. Corregir el tag de cierre incompleto — la línea 15 tiene </> en
   lugar de </body>.

Resultado final

<head>
  <script src="https://cdn.tailwindcss.com"></script>
  ...
</head>
<body class="bg-red-600">
  <h1>Hello World</h1>
</body>

¿Procedo con la implementación?
```

En este punto todavía estamos definiendo qué se debe hacer.

---

## 16. Pasar de Plan a Build

Para ejecutar el plan se debe cambiar al modo:

```text
Build
```

Presionar:

```text
Tab
```

hasta que aparezca seleccionado `Build`.

Luego se puede responder:

```text
si
```

OpenCode comienza a modificar los archivos.

Por ejemplo:

```text
← Edit index.html

3    <head>
4      <meta charset="UTF-8" />
5      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
6      <title>Document</title>
7 -    <style>
8 -      body {
9 -        background-color: #FF0000;
10-      }
11-    </style>
7 +    <script src="https://cdn.tailwindcss.com"></script>
8    </head>
13-   <body>
9 +   <body class="bg-red-600">
10     <h1>Hello World</h1>
15-   </>
11+   </body>
12   </html>


+ Thought: 130ms
→ Read index.html
+ Thought: 222ms


Listo. Tailwind CSS está instalado vía CDN en index.html:

- Línea 7: Script CDN de Tailwind agregado al <head>
- Línea 9: CSS inline reemplazado con la clase bg-red-600
- Línea 11: Tag de cierre </> corregido a </body>

Abre index.html en el navegador para verificar que el fondo rojo
sigue funcionando con Tailwind.
```

---

## 17. Continuar trabajando en Build

Una vez dentro de `Build`, se pueden realizar modificaciones adicionales directamente.

Por ejemplo:

```text
Cambia el color de fondo a morado pastel. Recuerda que usar Tailwind
```

OpenCode puede modificar directamente el archivo:

```text
Cambia el color de fondo a morado pastel. Recuerda que usar Tailwind

+ Thought: 1.4s
← Edit index.html

5      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
6      <title>Document</title>
7      <script src="https://cdn.tailwindcss.com"></script>
8    </head>
9 -  <body class="bg-red-600">
9 +  <body class="bg-purple-200">
10     <h1>Hello World</h1>
11   </body>
12 </html>

Fondo cambiado a bg-purple-200 (morado pastel) en index.html:9.
```

No es necesario regresar a `Plan` para cada modificación sencilla que ya esté contemplada en el plan. Si el ajuste cambia el alcance o requiere decisiones nuevas, vuelve a `Plan` y revisa la propuesta antes de continuar.

---

## 18. Resumen de atajos

| Acción                               | Comando               |
| ------------------------------------ | --------------------- |
| Mostrar comandos                     | `Ctrl + P`            |
| Borrar hasta el inicio de la línea   | `Ctrl + U`            |
| Borrar palabra anterior              | `Ctrl + W`            |
| Buscar/referenciar archivo           | `@archivo`            |
| Mostrar comandos `/`                 | `/`                   |
| Entrar en Shell                      | `!`                   |
| Salir del modo Shell                 | `Esc`                 |
| Crear nueva sesión                   | `/new`                |
| Recuperar sesión anterior al iniciar | `opencode -c`         |
| Mostrar sesiones                     | `Ctrl + X`, luego `L` |
| Renombrar sesión                     | `Ctrl + R`            |
| Mostrar/ocultar barra lateral        | `Ctrl + X`, luego `B` |
| Compactar contexto                   | `/compact`            |
| Deshacer                             | `/undo`               |
| Deshacer con teclado                 | `Ctrl + X`, luego `U` |
| Rehacer                              | `/redo`               |
| Rehacer con teclado                  | `Ctrl + X`, luego `R` |
| Cambiar entre Plan y Build           | `Tab`                 |
| Generar `AGENTS.md`                  | `/init`               |

---

## 19. Recorrido recomendado

1. Abre OpenCode desde la raíz del proyecto y documenta su objetivo antes de generar `AGENTS.md`.
2. Mantén las reglas globales en `AGENTS.md` y aporta archivos específicos con `@` cuando hagan falta.
3. Para tareas complejas, diseña y revisa el plan antes de pasar a `Build`; luego valida los cambios con las herramientas del proyecto.
4. Configura Git antes de depender de `/undo` y `/redo` para restaurar archivos. Antes de compactar una sesión, conserva las decisiones y restricciones importantes.

Para tareas con muchas decisiones, puede convenir un modelo de mayor capacidad durante `Plan` y uno más rápido durante `Build`, una vez que los pasos estén claros.
