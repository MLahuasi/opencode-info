# Configuración global y TUI de OpenCode

Esta guía describe dónde guarda OpenCode su configuración en Windows y cómo definir instrucciones globales y opciones de la interfaz de terminal (TUI).

## Ubicaciones de configuración y datos

Las rutas siguientes se recopilaron consultando la consola de OpenCode. `<usuario>` representa el nombre de la cuenta de Windows y `[proyecto]` la carpeta del proyecto.

| Nivel | Ruta | Contenido |
| --- | --- | --- |
| Configuración global | `C:\Users\<usuario>\.config\opencode\` | `opencode.jsonc`, `tui.json` y carpetas como `agents/`, `commands/`, `plugins/`, `skills/`, `themes/`, `tools/` y `modes/`. |
| Configuración del proyecto | `[proyecto]\opencode.json` o `[proyecto]\opencode.jsonc` | Ajustes del proyecto, como modelos, proveedores, permisos y MCP. Puede versionarse en Git. |
| Recursos del proyecto | `[proyecto]\.opencode\` | Agents, commands, plugins, skills y themes específicos del proyecto. |
| Datos y memoria | `C:\Users\<usuario>\.local\share\opencode\` | `opencode.db` (sesiones e historial), `auth.json`, logs y repositorios auxiliares. |
| Configuración administrada | `C:\ProgramData\opencode\opencode.json` | Configuración empresarial administrada por la organización. |

## Configurar instrucciones globales

En la carpeta global de configuración, crea `instructions.md` con las reglas que quieras aplicar a tus proyectos. Ejemplo:

```md
# Coding Instructions

Eres un agente de codificación enfocado en clean code.

- Responde de forma corta, clara y directa.
- Escribe el código en inglés: variables, funciones, clases, tipos y archivos.
- Escribe comentarios en español solo cuando aporten valor.
- Para funciones o APIs públicas, documenta descripción, parámetros y retorno cuando corresponda.
- Prioriza código simple, legible y fácil de mantener.
- Aplica DRY: evita duplicar lógica, validaciones, constantes o bloques de código; reutiliza solo cuando exista una duplicación real.
- Evita sobreingeniería y abstracciones innecesarias.
- Aplica SOLID cuando aporte valor.
- Prefiere código explícito y claro; evita lógica críptica y valores mágicos.
- Mantén funciones pequeñas y con una responsabilidad clara.
- Respeta la arquitectura y convenciones existentes del proyecto.
- Realiza solo los cambios necesarios para la tarea.
- No agregues dependencias sin necesidad.
- Aplica buenas prácticas de seguridad cuando corresponda.
```

Registra el archivo en `C:\Users\<usuario>\.config\opencode\opencode.jsonc`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["instructions.md"]
}
```

Si ya existe `opencode.jsonc`, agrega la propiedad `instructions` a su configuración en lugar de reemplazar el archivo.

## Configurar la TUI

El archivo global de la interfaz es:

```text
C:\Users\<usuario>\.config\opencode\tui.json
```

Puedes abrir el selector de temas desde OpenCode con:

```text
/theme
```

Si el archivo `tui.json` no se crea automáticamente, créalo en la ruta anterior. Esta configuración define un tema y opciones de atención, notificaciones y sonido:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "tokyonight",
  "attention": {
    "enabled": true,
    "notifications": true,
    "sound": true,
    "volume": 0.4,
    "sound_pack": "opencode.default"
  }
}
```

Opciones incluidas:

- `theme`: tema visual de OpenCode.
- `attention.enabled`: activa las opciones de atención.
- `attention.notifications`: habilita notificaciones.
- `attention.sound`: habilita sonidos.
- `attention.volume`: volumen entre `0` y `1`.
- `attention.sound_pack`: paquete de sonidos, aquí `opencode.default`.

Para consultar otras opciones, revisa la [documentación de configuración de la TUI](https://opencode.ai/docs/es/tui/#configurar). No es necesario definir `sounds` salvo que quieras personalizar archivos de audio específicos.

## Aplicar los cambios

Reinicia OpenCode después de modificar `opencode.jsonc`, `instructions.md` o `tui.json`. Para continuar la conversación anterior al iniciar, ejecuta:

```bash
opencode -c
```
