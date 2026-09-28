# Configurar proveedores en OpenCode

Esta guía reúne tres formas de conectar modelos a OpenCode: OpenCode Zen mediante una API key, OpenAI mediante autenticación de ChatGPT en el navegador y Ollama mediante su comando de inicio.

## Preparar el espacio de trabajo

Abre PowerShell, crea o elige una carpeta de trabajo e inicia OpenCode desde ella:

```powershell
mkdir <ruta-de-trabajo>\open-code
cd <ruta-de-trabajo>\open-code
opencode
```

Reemplaza `<ruta-de-trabajo>` por la ubicación deseada. Para conectar proveedores desde la interfaz de OpenCode, ejecuta:

```text
/connect
```

La lista de proveedores disponibles depende de la versión instalada. En los ejemplos de esta guía aparecen OpenCode Zen, OpenAI y Ollama.

## Opción 1: OpenCode Zen

1. En OpenCode, ejecuta `/connect` y selecciona **OpenCode Zen**.
2. Abre el enlace que muestra la interfaz —`https://opencode.ai/zen`— e inicia sesión o crea una cuenta. En el flujo documentado se podía usar Google o GitHub.
3. En **Claves API**, crea una API key o copia una existente.
4. Vuelve a OpenCode, pega la clave en el campo **API key** y confirma con `Enter`.
5. Selecciona un modelo disponible. `Big Pickle` se usó como ejemplo gratuito para una prueba básica.
6. Inicia una conversación y envía `ping`. Una respuesta como `pong` confirma que el modelo respondió.

> No guardes API keys reales en repositorios, documentación ni archivos públicos.

## Opción 2: OpenAI mediante una cuenta ChatGPT Plus o Pro

1. En OpenCode, ejecuta `/connect` y selecciona **OpenAI (ChatGPT Plus/Pro or API key)**.
2. Selecciona **ChatGPT Pro/Plus (browser)** y abre el enlace de autenticación que muestra OpenCode.
3. Inicia sesión en ChatGPT y selecciona el espacio de trabajo que corresponda, por ejemplo **Personal**.
4. Regresa a OpenCode. Al completarse la autorización, la interfaz mostrará los modelos disponibles para esa cuenta.
5. Selecciona un modelo e inicia una conversación para comprobar que responde.

Los nombres de modelos que se observaron durante la configuración incluían `GPT-5.6 Luna`, `GPT-5.6 Luna Fast`, `GPT-5.6 Sol` y `GPT-5.6 Sol Fast`. La disponibilidad y los nombres pueden cambiar.

## Opción 3: Ollama

Ollama permite iniciar OpenCode con modelos compatibles, incluidos modelos locales.

1. Instala la aplicación de Ollama correspondiente a tu sistema operativo.
2. Comprueba que el comando esté disponible:

   ```bash
   ollama --version
   ```

3. Busca un modelo en la biblioteca de Ollama. Por ejemplo: <https://ollama.com/library/qwen3.5>.
4. Desde una terminal, ejecuta el comando indicado por el modelo. El formato general es:

   ```bash
   ollama launch opencode --model <modelo>
   ```

   Ejemplos documentados:

   ```bash
   ollama launch opencode --model qwen3.5
   ollama launch opencode --model granite3.3:2b
   ollama launch opencode --model hermes3:3b
   ollama launch opencode --model qwen2.5-coder:3b
   ```

5. Dentro de OpenCode, ejecuta `/models` y comprueba que el modelo iniciado aparezca disponible.

## Consultar y cambiar de modelo

En OpenCode, ejecuta:

```text
/models
```

La lista permite consultar los modelos disponibles para los proveedores configurados y seleccionar otro.

## Crear una conversación nueva

Presiona `Ctrl + X` y luego `N`:

```text
Ctrl + X
N
```

El atajo `Ctrl + X` también da acceso a otras acciones de OpenCode; las opciones disponibles pueden consultarse desde la interfaz.

## Resumen del flujo

Para Zen y OpenAI, inicia OpenCode y configura la autenticación desde `/connect`. Para Ollama, instala Ollama y ejecuta `ollama launch opencode --model <modelo>`. En cualquiera de los casos, usa `/models` para revisar la selección de modelo e inicia una conversación para validar la respuesta.
