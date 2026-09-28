# Configuración global y TUI de OpenCode

## 1. Ubicaciones de configuración y memoria

> Nota: esta información se obtuvo consultando directamente la consola de OpenCode sobre las ubicaciones donde almacena configuración y memoria.

| Nivel                      | Ruta en Windows                                          | ¿Qué almacena?                                                                                                                                       |
| -------------------------- | -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Global (config)**        | `C:\Users\<usuario>\.config\opencode\`                   | Configuración global: `opencode.jsonc`, `tui.json` y subcarpetas como `agents/`, `commands/`, `plugins/`, `skills/`, `themes/`, `tools/` y `modes/`. |
| **Proyecto (config)**      | `[proyecto]\opencode.json` o `[proyecto]\opencode.jsonc` | Configuración específica del proyecto: modelos, providers, permisos, MCP, etc. Puede versionarse en Git.                                             |
| **Proyecto (`.opencode`)** | `[proyecto]\.opencode\`                                  | Agents, commands, plugins, skills y themes propios del proyecto.                                                                                     |
| **Datos / memoria**        | `C:\Users\<usuario>\.local\share\opencode\`              | `opencode.db` con sesiones e historial, `auth.json`, logs y repositorios auxiliares.                                                                 |
| **Managed**                | `C:\ProgramData\opencode\opencode.json`                  | Configuración empresarial administrada por la organización.                                                                                          |

---

## 2. Configuración global

Ir a:

```text
C:\Users\<usuario>\.config\opencode\opencode.jsonc
```

Configuración inicial:

```json
{
  "$schema": "https://opencode.ai/config.json"
}
```

---

## 3. Instrucciones globales

En el mismo directorio crear:

```text
instructions.md
```

Contenido:

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

Registrar el archivo en `opencode.jsonc`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["instructions.md"]
}
```

---

## 4. Configuración de la TUI

El archivo global de configuración de la interfaz se encuentra en:

```text
C:\Users\<usuario>\.config\opencode\tui.json
```

Para intentar que OpenCode lo cree automáticamente, cambiar el tema desde la consola:

```text
/theme
```

Si `tui.json` no se crea, hacerlo manualmente.

Configuración mínima:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "opencode"
}
```

---

## 5. Configurar tema, notificaciones y sonidos

Para consultar las opciones disponibles:

1. Ir a `https://opencode.ai/`
2. Abrir **Documentación**.
3. Ir a **TUI**.
4. Revisar la sección de configuración:

```text
https://opencode.ai/docs/es/tui/#configurar
```

Configuración funcional:

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

Opciones principales:

- `theme`: tema visual de OpenCode.
- `enabled`: activa el sistema de atención.
- `notifications`: habilita notificaciones.
- `sound`: habilita sonidos.
- `volume`: volumen entre `0` y `1`.
- `sound_pack`: paquete de sonidos utilizado por OpenCode.

Para habilitar correctamente los sonidos se usa:

```json
"sound_pack": "opencode.default"
```

No es necesario definir `sounds` salvo que se quieran personalizar archivos de audio específicos.

---

## 6. Reiniciar OpenCode

Después de modificar `opencode.jsonc`, `instructions.md` o `tui.json`, reiniciar OpenCode.

Para continuar la conversación anterior:

```bash
opencode -c
```
