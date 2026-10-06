# Iniciar OpenCode y editar el prompt

> **Ejemplo:** los comandos y atajos de este documento sirven para dar contexto. Confirma su disponibilidad con `/` o `Ctrl + P` en tu versión de OpenCode.

## 1. Iniciar OpenCode

Una vez instalado OpenCode, puede utilizarse desde PowerShell, Warp, CMD u otra terminal compatible.

Primero ingresa al directorio del proyecto:

```bash
cd <directorio-del-proyecto>
```

Luego ejecuta:

```bash
opencode
```

OpenCode utilizará el directorio actual como proyecto de trabajo.

## 2. Mostrar comandos

Escribe `/` para consultar y filtrar los comandos disponibles en la sesión. También puedes utilizar:

```text
Ctrl + P
```

La lista puede variar según la versión y la configuración.

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

Usa `/` para confirmar qué comandos están disponibles en tu sesión.

## 3. Editar el prompt

Para borrar desde el cursor hasta el inicio de la línea:

```text
Ctrl + U
```

Para borrar la palabra anterior:

```text
Ctrl + W
```

## 4. Referenciar archivos con `@`

El carácter `@` permite buscar y referenciar archivos dentro del prompt. Por ejemplo:

```text
@index.html
```

También puede utilizarse para aportar documentación específica a una solicitud:

```text
@naming_rules.md
@architecture_rules.md
```

La referencia incorpora el archivo a la solicitud correspondiente; no implica que sus reglas queden cargadas permanentemente en todas las sesiones.

---

[← Anterior](../05_opencode-context.md) | [Temario](../06_opencode_tui.md) | [Siguiente →](02-sesiones-y-panel-lateral.md)
