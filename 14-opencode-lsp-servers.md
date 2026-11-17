# LSP Servers

Es un estándar abierto creado por `Microsoft` para la comunicación entre un editor de código (como `Visual Studio Code`, `Neovim` o `OpenCode`) y un `servidor de lenguaje`.

Separa la inteligencia de programación del editor. En lugar de que cada editor cree herramientas desde cero para cada lenguaje, un servidor LSP analiza el código y le proporciona al editor funciones como autocompletado, detección de errores, ir a la definición (Go to definition) y refactorización.

Consulta la [documentación oficial de LSP Servers](https://opencode.ai/docs/lsp/) para conocer las opciones disponibles.

---

## 1. Activar

Añadir en [opencode.jsonc](../open-daycare/opencode.json) `lsp` para activar.

```json
{
  "lsp": true
}
```

**NOTA**: Activar LSP aumenta el contexto
