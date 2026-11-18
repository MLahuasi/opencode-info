# Formatters en OpenCode

OpenCode puede ejecutar un formatter después de que las herramientas `write`, `edit` o `patch` modifiquen un archivo. Los formatters están desactivados por defecto y se configuran en `opencode.json` u `opencode.jsonc`.

Consulta la [documentación oficial de Formatters](https://opencode.ai/v2/docs/formatters) y la [referencia de configuración](https://opencode.ai/v2/docs/config) para conocer las opciones vigentes.

> **Contexto de los ejemplos:** las configuraciones y comandos de este documento son ejemplos. Adáptalos al lenguaje, las dependencias y las convenciones del proyecto.

## 1. Activar los formatters integrados

Para activar los formatters disponibles, agrega `formatter: true` a la configuración del proyecto:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": true,
}
```

OpenCode ejecuta un formatter integrado únicamente cuando encuentra el ejecutable y los requisitos específicos del proyecto.

También puedes utilizar un objeto vacío para habilitar los formatters integrados y conservar una configuración heredada:

```jsonc
{
  "formatter": {},
}
```

## 2. Formatters integrados

OpenCode incluye formatters para distintos lenguajes y extensiones. Algunos ejemplos son:

| Formatter | Extensiones o archivos | Requisito principal |
| --- | --- | --- |
| `gofmt` | `.go` | Comando `gofmt` |
| `prettier` | JavaScript, TypeScript, HTML, CSS, JSON, YAML, Markdown y otros | Dependencia `prettier` en `package.json` |
| `biome` | Extensiones compatibles con Prettier | `biome.json` o `biome.jsonc` y el binario de Biome |
| `ruff` | `.py`, `.pyi` | Comando `ruff` y configuración de Ruff |
| `rustfmt` | `.rs` | Comando `rustfmt` |
| `shfmt` | `.sh`, `.bash` | Comando `shfmt` |
| `terraform` | `.tf`, `.tfvars` | Comando `terraform` |

La disponibilidad real depende de los ejecutables, las dependencias y los archivos de configuración presentes en el proyecto.

## 3. Personalizar un formatter

Puedes sobrescribir un formatter integrado o definir uno propio. Un formatter personalizado necesita `command` y `extensions`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": {
    "prettier": {
      "environment": {
        "NODE_ENV": "development",
      },
      "extensions": [".js", ".ts"],
    },
    "deno-markdown": {
      "command": ["deno", "fmt", "$FILE"],
      "extensions": [".md"],
    },
  },
}
```

Las propiedades disponibles son:

- `disabled`: desactiva el formatter cuando vale `true`.
- `command`: arreglo de argumentos que reemplaza el comando integrado o define uno nuevo.
- `environment`: variables de entorno adicionales.
- `extensions`: extensiones que puede procesar, incluyendo el punto inicial.

## 4. Comandos y variable `$FILE`

`command` debe ser un arreglo de argumentos, no una cadena de comandos del shell. OpenCode reemplaza `$FILE` por la ruta absoluta del archivo y ejecuta el comando desde el directorio activo del proyecto.

Ejemplo:

```jsonc
{
  "formatter": {
    "custom": {
      "command": ["custom-fmt", "--write", "$FILE"],
      "extensions": [".custom"],
    },
  },
}
```

## 5. Coincidencia entre archivos y formatters

OpenCode compara la extensión final del archivo con los valores de `extensions`. La coincidencia distingue mayúsculas y minúsculas. Por ejemplo, `.md` coincide con `notes.md`, pero una entrada `.part.md` no coincide con `notes.part.md` porque su extensión final es `.md`.

Si varios formatters coinciden, OpenCode los prueba en el orden registrado y detiene el proceso después del primer comando que termina correctamente. Si uno falla, registra el error y prueba el siguiente formatter coincidente.

## 6. Desactivar formatters

Para desactivar globalmente los formatters configurados por un archivo de menor prioridad, establece `formatter` en `false`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": false,
}
```

Para desactivar únicamente un formatter integrado y mantener los demás activos:

```jsonc
{
  "formatter": {
    "prettier": {
      "disabled": true,
    },
  },
}
```

## 7. Recomendaciones

- Instala y configura el formatter antes de activar la automatización.
- Comprueba que el comando funcione desde la terminal del proyecto.
- Revisa los cambios generados por el formatter antes de crear un commit.
- Evita configurar varios formatters para las mismas extensiones salvo que necesites un fallback.
- Mantén la configuración del formatter junto con las dependencias y reglas del proyecto.

---

[← Anterior](14-opencode-lsp-servers.md) | [Índice principal](README.md) | [Siguiente →](16-opencode-subagents.md)
