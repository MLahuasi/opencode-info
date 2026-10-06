# Git, Undo y Redo

> **Ejemplo:** los comandos y mensajes de commit son ilustrativos. Revisa el estado del repositorio y sus reglas antes de ejecutarlos.

## 1. Preparar Git para Undo y Redo

Para que `/undo` y `/redo` puedan restaurar cambios realizados sobre archivos, el proyecto debe tener un estado Git utilizable. El flujo completo de Git, ramas y colaboración se encuentra en [Git y GitHub](../08-opencode-github.md).

Es recomendable realizar esta configuración desde la terminal normal para no utilizar tokens innecesariamente.

### 1.1. Inicializar Git

Desde el directorio del proyecto:

```bash
git init
```

### 1.2. Revisar y agregar archivos

Antes de agregar archivos, revisa qué se incluirá y confirma que los secretos estén excluidos en `.gitignore`:

```bash
git status
git diff
git add <rutas-revisadas>
git diff --cached
```

### 1.3. Crear un estado inicial

Si el repositorio todavía no tiene un commit y corresponde crear un punto base:

```bash
git commit -m "Create initial repository state"
```

El commit es un ejemplo de estado de referencia. Puede fallar si Git no tiene configurado el nombre o correo del autor, o si el repositorio ya tiene historial.

### 1.4. Reiniciar OpenCode

Después de configurar Git, inicia OpenCode desde el directorio del proyecto. Si ya estaba abierto y no reconoce el repositorio, reinícialo:

```bash
opencode
```

Los comandos `/undo` y `/redo`, junto con sus atajos, pueden utilizar el estado Git disponible para revertir o restaurar cambios de archivos.

## 2. Undo

`/undo` revierte el último mensaje del usuario y las respuestas posteriores. La restauración de archivos depende de que OpenCode pueda utilizar un estado Git válido.

```text
/undo
```

También se puede utilizar:

```text
Ctrl + X
U
```

## 3. Redo

`/redo` restaura cambios revertidos por `/undo` cuando existe una acción que pueda rehacerse:

```text
/redo
```

También se puede utilizar:

```text
Ctrl + X
R
```

Sin Git, no dependas de Undo/Redo para recuperar cambios en archivos. Revisa siempre el diff y conserva los cambios importantes en commits o archivos de respaldo según el flujo del proyecto.

---

[← Anterior](04-modo-shell.md) | [Temario](../06_opencode_tui.md) | [Siguiente →](06-init-agents-e-ignorados.md)
