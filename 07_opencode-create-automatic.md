# [Creación automática usando OpenCode](https://github.com/MLahuasi/opencode-weather-cli-app)

> **NOTA:** Esta opción no es necesariamente la más recomendada para aprender, ya que OpenCode puede generar una cantidad considerable de código, configuraciones o funcionalidades que todavía no comprendamos completamente.
>
> Cada cambio generado debe ser revisado, comprendido y validado antes de incorporarlo al proyecto.

Una forma práctica de trabajar con OpenCode consiste en preparar primero el proyecto y su documentación, utilizar `Plan` para diseñar los cambios y posteriormente utilizar `Build` para implementarlos.

El flujo general es:

```text
Documentar requerimientos
        ↓
Crear/configurar proyecto
        ↓
Generar AGENTS.md
        ↓
Plan
        ↓
Revisar plan
        ↓
Build
        ↓
Revisar implementación
        ↓
Testing
        ↓
Git
        ↓
CI/CD
```

---

# 1. Preparar el proyecto

## 1.1. Documentar los requerimientos

Antes de solicitar la implementación es recomendable documentar claramente qué debe hacer la aplicación.

El archivo principal puede ser:

```text
README.md
```

También se pueden agregar documentos adicionales con instrucciones específicas, por ejemplo:

```text
bun-instructions.md
revision.md
architecture.md
```

La documentación debería definir, al menos:

- Objetivo de la aplicación.
- Tecnologías que se deben utilizar.
- Funcionalidades principales.
- Restricciones.
- Convenciones del proyecto.
- Comportamientos esperados.
- Comandos importantes.
- Requisitos de testing.
- Consideraciones de arquitectura.

Mientras más claras sean las instrucciones, menor será la cantidad de decisiones que OpenCode tendrá que asumir.

---

## 1.2. Crear manualmente la estructura inicial

Cuando sea posible, es preferible ejecutar manualmente los comandos relacionados con la creación y configuración inicial del proyecto.

Por ejemplo, usando Bun:

```bash
bun init
```

Esto permite:

- Comprender la estructura inicial.
- Controlar las dependencias.
- Evitar configuraciones innecesarias.
- Saber exactamente qué archivos fueron creados.
- Reducir la cantidad de decisiones delegadas al agente.

---

## 1.3. Configurar los scripts

Los scripts principales también pueden configurarse manualmente.

Por ejemplo:

```json
{
  "scripts": {
    "build": "bun build --compile src/index.ts --outfile weather",
    "start": "bun run src/index.ts",
    "dev": "bun run src/index.ts --watch",
    "test": "bun test"
  }
}
```

Antes de delegar tareas a OpenCode conviene comprobar que los comandos básicos del proyecto funcionen correctamente.

---

# 2. Generar `AGENTS.md`

Una vez preparada la estructura inicial y la documentación del proyecto, ejecutar OpenCode dentro del repositorio y utilizar:

```text
/init
```

OpenCode puede generar:

```text
AGENTS.md
```

> **IMPORTANTE:** `AGENTS.md` contiene instrucciones que OpenCode utilizará para comprender cómo debe trabajar dentro del proyecto.

Después de generarlo, se debe revisar cuidadosamente.

Puede modificarse manualmente para:

- Agregar reglas.
- Eliminar instrucciones innecesarias.
- Definir convenciones.
- Establecer restricciones.
- Especificar comandos de validación.
- Documentar la arquitectura.
- Definir la ubicación de archivos.
- Indicar qué archivos no deben modificarse.
- Definir cómo ejecutar tests.
- Establecer criterios de aceptación.

`AGENTS.md` no debe considerarse correcto únicamente porque fue generado automáticamente.

---

# 3. Diseñar la implementación con `Plan`

Para desarrollar una funcionalidad es recomendable comenzar trabajando en:

```text
Plan
```

El modo puede cambiarse utilizando:

```text
TAB
```

En `Plan`, OpenCode analiza la solicitud y propone una estrategia antes de comenzar a modificar el proyecto.

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

---

# 4. Revisar el plan

Durante la planificación OpenCode puede solicitar información adicional o presentar diferentes alternativas.

Por ejemplo, puede preguntar sobre:

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

1. Seleccionar una de las alternativas.
2. Ingresar una respuesta personalizada.
3. Solicitar otra alternativa.
4. Pedir cambios al plan.

> **IMPORTANTE:** Si el plan no cumple con lo esperado, se deben solicitar ajustes antes de comenzar la implementación.

No es recomendable pasar a `Build` hasta comprender y aprobar el plan propuesto.

---

# 5. Implementar usando `Build`

Una vez revisado y aprobado el plan, cambiar a:

```text
Build
```

Utilizando nuevamente:

```text
TAB
```

Después solicitar la implementación:

```text
Implementa el plan diseñado.
```

Si el plan está suficientemente detallado, durante `Build` puede utilizarse un modelo de menor capacidad para reducir el consumo de tokens.

La idea es:

```text
Plan  → mayor razonamiento
Build → ejecución del plan
```

Esto no significa que `Build` no requiera razonamiento, sino que gran parte de las decisiones importantes deberían haberse resuelto previamente.

---

# 6. Revisar la implementación

Después de cada implementación se deben analizar los archivos modificados.

Es importante revisar:

- Código generado.
- Arquitectura.
- Dependencias instaladas.
- Configuraciones modificadas.
- Manejo de errores.
- Testing.
- Scripts.
- Comportamiento de la aplicación.
- Archivos nuevos.
- Archivos eliminados.
- Cambios no solicitados.

También es recomendable verificar:

```bash
bun run build
bun test
```

Los comandos concretos dependerán de cada proyecto.

> No se debe asumir que una implementación es correcta únicamente porque compila.

---

# 7. Realizar ajustes posteriores

Si después de revisar la implementación se requieren cambios adicionales, se puede documentar la revisión en un archivo:

```text
revision.md
```

Por ejemplo:

```text
Revisa los cambios solicitados en @revision.md y diseña
un plan para implementarlos.
```

Para cambios importantes es recomendable repetir siempre el flujo:

```text
Plan
  ↓
Revisar plan
  ↓
Build
  ↓
Revisar implementación
  ↓
Testing
```

---

# 8. Control de versiones con Git

Git es fundamental cuando se trabaja con agentes que pueden modificar múltiples archivos automáticamente.

Antes de implementar una funcionalidad importante es recomendable crear una rama.

Por ejemplo:

```bash
git checkout -b feature/colors
```

El flujo recomendado es:

```text
main
  │
  ├── crear rama
  │
  ▼
feature/*
  │
  ├── implementar
  ├── probar
  ├── revisar
  └── commit
  │
  ▼
main
  │
  └── merge
```

Una vez que los cambios fueron revisados y aprobados:

```bash
git add .

git commit -m "Implement feature"
```

Regresar a la rama principal:

```bash
git checkout main
```

Integrar los cambios:

```bash
git merge feature/colors
```

Si existen conflictos durante el `merge`, deberán revisarse y resolverse manualmente.

Finalmente:

```bash
git branch -d feature/colors
```

---

# 9. Revertir cambios generados por OpenCode

Si OpenCode realizó modificaciones que se quieren descartar antes de realizar un commit, Git permite recuperar el estado anterior.

Para restaurar archivos modificados o eliminados:

```bash
git restore .
```

Para eliminar archivos y directorios nuevos que todavía no están registrados por Git:

```bash
git clean -fd
```

> **IMPORTANTE:** `git clean -fd` elimina archivos y directorios no rastreados. Antes de ejecutarlo se debe comprobar qué se eliminará.

Puede realizarse primero una simulación:

```bash
git clean -fdn
```

Una estrategia segura es:

```bash
git status
git diff
git clean -fdn
```

y únicamente después decidir si se descartan los cambios.

---

# 10. Publicar el proyecto en GitHub

Una vez creado el repositorio remoto, se puede vincular el proyecto local.

Ejemplo:

```bash
git remote add origin https://github.com/<usuario>/<repositorio>.git

git branch -M main

git push -u origin main
```

Se debe reemplazar:

```text
<usuario>
<repositorio>
```

por los valores correspondientes.

---

# 11. Pull Requests

Aunque es posible integrar cambios directamente mediante `merge`, para proyectos importantes es recomendable utilizar Pull Requests.

Un flujo más controlado sería:

```text
main
  │
  └── feature/*
        │
        ├── implementación
        ├── testing
        ├── commit
        ├── push
        │
        ▼
   Pull Request
        │
        ├── revisión
        ├── CI
        └── aprobación
        │
        ▼
       main
```

Los Pull Requests permiten:

- Revisar cambios antes del merge.
- Ejecutar GitHub Actions.
- Detectar errores mediante CI.
- Mantener un historial de decisiones.
- Facilitar revisiones de código.

---

# 12. Pruebas automáticas

El testing debe formar parte del flujo de desarrollo y no ser una tarea opcional posterior.

Para implementar pruebas en una funcionalidad separada se puede crear una rama:

```bash
git checkout -b feature/testing
```

Después, en `Plan`, se puede solicitar:

```text
Desarrolla un plan para implementar testing automático usando Bun.

Requisitos:

- Todos los tests deben almacenarse en ./tests/.
- La estructura de ./tests/ debe replicar, cuando tenga sentido,
  la estructura existente dentro de ./src/.
- Usa las herramientas de testing incluidas con Bun.
- No agregues dependencias de testing si no son necesarias.
- La aplicación no debe construirse ni publicarse si los tests fallan.
- Revisa primero la estructura actual del proyecto antes de proponer cambios.
```

Después de revisar el plan:

```text
Build
```

y solicitar:

```text
Implementa el plan diseñado.
```

Los tests pueden ejecutarse mediante:

```bash
bun test
```

Antes de aprobar los cambios se debe comprobar:

- Qué funcionalidades fueron probadas.
- Qué casos de error fueron cubiertos.
- Si existen tests innecesariamente acoplados a la implementación.
- Si los tests pueden ejecutarse de forma independiente.
- Si un fallo realmente provoca que el proceso de CI se detenga.

---

# 13. GitHub Actions

GitHub Actions puede utilizarse para automatizar:

- Instalación de dependencias.
- Testing.
- Build.
- Validaciones.
- Generación de ejecutables.
- Creación de releases.

La configuración normalmente se almacena en:

```text
.github/
└── workflows/
    └── release.yml
```

La estructura puede crearse manualmente:

```text
.github
.github/workflows
```

Posteriormente se puede solicitar a OpenCode que diseñe el workflow.

Es preferible comenzar en `Plan`.

Ejemplo:

```text
Diseña un GitHub Action para publicar una nueva versión
de la aplicación.

Requisitos:

- Ejecutarse cuando corresponda publicar una nueva versión.
- Instalar las dependencias.
- Ejecutar los tests antes del build.
- Detener el workflow si algún test falla.
- Obtener la versión desde @package.json.
- Ejecutar el comando de build definido por el proyecto.
- Generar los ejecutables soportados.
- Crear un GitHub Release asociado a la versión.
- Adjuntar los ejecutables al release.
- Revisar las variables requeridas tomando como referencia
  @.env.example.

No implementes todavía. Primero presenta el plan.
```

Después de revisar y aprobar el plan se puede pasar a `Build`.

---

# 14. Validar GitHub Actions

Después de crear o modificar el workflow:

```bash
git add .

git commit -m "Configure GitHub Actions release workflow"

git push
```

El workflow puede revisarse en:

```text
GitHub
└── Actions
```

Si falla, se deben analizar los logs antes de realizar nuevas modificaciones.

Algunas causas frecuentes son:

- Tests fallidos.
- Error de compilación.
- Dependencias incorrectas.
- Variables faltantes.
- Permisos insuficientes.
- Problemas con el workflow.
- Diferencias entre sistemas operativos.
- Comandos que funcionan localmente pero no en CI.

> **IMPORTANTE:** No se debe modificar el workflow simplemente hasta conseguir que aparezca en verde. Primero debe identificarse la causa real del fallo.

---

# 15. Releases y ejecutables

Si el workflow finaliza correctamente, puede crear automáticamente un GitHub Release.

Un release multiplataforma podría contener archivos como:

```text
weather-linux-x64
weather-macos-arm64
weather-windows-x64.exe
Source code (zip)
Source code (tar.gz)
```

Los nombres y arquitecturas dependerán de los sistemas operativos soportados por el proyecto.

Archivos como:

```text
.env.example
```

pueden incluirse como documentación si son necesarios, pero nunca deben contener secretos reales.

---

# 16. Seguridad de los entregables

Los ejecutables generados deben considerarse artefactos sensibles de distribución.

Al publicar binarios se deben revisar aspectos como:

- Código incluido.
- Dependencias.
- Variables de entorno.
- Secretos.
- Archivos empaquetados accidentalmente.
- Plataforma y arquitectura.
- Procedencia del build.
- Integridad del artefacto.

> **IMPORTANTE:** Nunca deben incluirse archivos `.env` reales, claves API, tokens, credenciales o secretos dentro de los ejecutables o releases.

Un ejecutable descargado desde Internet también puede generar advertencias de seguridad del sistema operativo, especialmente cuando no está firmado digitalmente.

---

# 17. Flujo completo recomendado

El proceso completo puede resumirse así:

```text
README.md / documentación
          ↓
Configuración inicial manual
          ↓
       /init
          ↓
      AGENTS.md
          ↓
      Revisar reglas
          ↓
       Crear rama
          ↓
         Plan
          ↓
     Revisar plan
          ↓
         Build
          ↓
    Revisar código
          ↓
       Testing
          ↓
        Commit
          ↓
         Push
          ↓
   Pull Request / CI
          ↓
        Merge
          ↓
   GitHub Actions
          ↓
       Release
```

---

# 18. Checklist de revisión

Antes de aprobar una implementación generada por OpenCode comprobar:

## Código

- [ ] ¿Se entiende el código generado?
- [ ] ¿Se comprende qué hace cada componente importante?
- [ ] ¿Se conocen las limitaciones de la implementación?
- [ ] ¿Se agregaron funcionalidades que no fueron solicitadas?
- [ ] ¿La arquitectura sigue siendo coherente?
- [ ] ¿El código puede simplificarse?

## Dependencias

- [ ] ¿OpenCode instaló nuevas dependencias?
- [ ] ¿Qué dependencias instaló?
- [ ] ¿Son realmente necesarias?
- [ ] ¿Existe una solución utilizando herramientas ya disponibles?

## Configuración

- [ ] ¿Se modificaron archivos de configuración?
- [ ] ¿Los cambios eran necesarios?
- [ ] ¿Se agregaron variables de entorno?
- [ ] ¿Se evitaron secretos dentro del repositorio?

## Testing

- [ ] ¿Se ejecutaron los tests?
- [ ] ¿Qué comportamiento cubren?
- [ ] ¿Se probaron casos de error?
- [ ] ¿La nueva funcionalidad rompió otra existente?
- [ ] ¿El CI falla correctamente cuando un test falla?

## Build

- [ ] ¿La aplicación compila correctamente?
- [ ] ¿La aplicación se ejecutó manualmente?
- [ ] ¿Los ejecutables generados funcionan?
- [ ] ¿Se probaron las plataformas soportadas?

## Git

- [ ] ¿Se creó una rama antes de realizar cambios importantes?
- [ ] ¿Los cambios fueron revisados antes del `merge`?
- [ ] ¿Se revisó `git diff`?
- [ ] ¿El commit representa correctamente los cambios?
- [ ] ¿Sería conveniente utilizar un Pull Request?

## GitHub Actions

- [ ] ¿El workflow ejecuta los tests antes de publicar?
- [ ] ¿El workflow se detiene ante errores?
- [ ] ¿Los permisos utilizados son los mínimos necesarios?
- [ ] ¿Los releases contienen únicamente los archivos esperados?
- [ ] ¿No existen secretos dentro de los artefactos?

---

> **Regla principal:** OpenCode puede acelerar considerablemente el desarrollo, pero la responsabilidad sobre las decisiones, el código, las dependencias, las pruebas, la seguridad y los artefactos publicados continúa siendo del desarrollador.
