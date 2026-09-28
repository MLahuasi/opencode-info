# Configurar proveedores en OpenCode

Esta guía explica cómo conectar distintos proveedores y modelos de IA en OpenCode.

Se incluyen tres escenarios:

- OpenCode Zen mediante API key.
- OpenAI mediante una cuenta de ChatGPT Plus o Pro.
- Ollama para usar modelos locales o compatibles con Ollama.

---

## 1. Preparar un directorio de trabajo

Crear un directorio dedicado para trabajar con OpenCode.

```powershell
mkdir <ruta-de-trabajo>\open-code
```

Ingresar al directorio:

```powershell
cd <ruta-de-trabajo>\open-code
```

Iniciar OpenCode:

```powershell
opencode
```

Esto abrirá la interfaz de consola.

---

# Conectar un proveedor desde OpenCode

Dentro de OpenCode ejecutar:

```text
/connect
```

Presionar `Enter`.

OpenCode abrirá la opción:

```text
Connect a provider
```

y mostrará los proveedores disponibles.

Entre ellos pueden aparecer:

```text
OpenCode Zen
OpenCode Go
OpenAI
GitHub Copilot
Anthropic
Google
...
```

La lista puede variar según la versión de OpenCode.

---

# Opción 1: OpenCode Zen

## 1. Seleccionar el proveedor

Dentro de:

```text
/connect
```

seleccionar:

```text
OpenCode Zen
```

Presionar `Enter`.

OpenCode solicitará una API key.

---

## 2. Obtener la API key

OpenCode mostrará el enlace:

```text
https://opencode.ai/zen
```

Abrirlo en el navegador.

La plataforma solicitará crear una cuenta o iniciar sesión.

Se puede utilizar, por ejemplo:

- Google
- GitHub

Una vez autenticado:

1. Ingresar a **Claves API**.
2. Crear una nueva API key o utilizar una clave existente.
3. Copiar la API key.

> No guardar API keys reales en repositorios, documentación ni archivos públicos.

---

## 3. Registrar la API key

Regresar a la consola de OpenCode.

Pegar la API key en el campo:

```text
API key
```

Presionar:

```text
Enter
```

OpenCode configurará el proveedor.

---

## 4. Seleccionar un modelo

Después de configurar el proveedor, seleccionar uno de los modelos disponibles.

Para una prueba básica se puede seleccionar un modelo gratuito, por ejemplo:

```text
Big Pickle
```

---

## 5. Validar la conexión

Crear una conversación sencilla.

Por ejemplo:

```text
ping
```

Una respuesta válida puede ser:

```text
pong
```

Si el modelo responde, la conexión quedó configurada correctamente.

---

# Opción 2: OpenAI con ChatGPT Plus o Pro

## 1. Abrir la configuración de proveedores

Dentro de OpenCode ejecutar:

```text
/connect
```

Presionar `Enter`.

---

## 2. Seleccionar OpenAI

Seleccionar:

```text
OpenAI (ChatGPT Plus/Pro or API key)
```

Presionar `Enter`.

---

## 3. Seleccionar autenticación mediante navegador

Seleccionar:

```text
ChatGPT Pro/Plus (browser)
```

OpenCode mostrará un enlace de autenticación.

Abrirlo en el navegador.

---

## 4. Autenticarse en ChatGPT

En el navegador:

1. Iniciar sesión o seleccionar la cuenta de ChatGPT.
2. Crear una cuenta si es necesario.
3. Seleccionar el espacio de trabajo correspondiente.

Por ejemplo:

```text
Personal
```

o cualquier otro workspace disponible.

---

## 5. Regresar a OpenCode

Después de completar la autorización en el navegador, regresar a la consola.

OpenCode detectará la autenticación y mostrará los modelos disponibles para la cuenta.

---

## 6. Seleccionar un modelo

Según los modelos disponibles en ese momento, pueden aparecer opciones como:

```text
GPT-5.6 Luna
GPT-5.6 Luna Fast
GPT-5.6 Sol
GPT-5.6 Sol Fast
...
```

Seleccionar el modelo que se desea utilizar.

> Los modelos disponibles pueden cambiar con el tiempo.

---

## 7. Validar la conexión

Crear una conversación sencilla.

Por ejemplo:

```text
ping
```

Si el modelo responde correctamente, OpenAI quedó conectado a OpenCode.

---

# Opción 3: Ollama

Ollama permite utilizar modelos locales o modelos disponibles mediante su plataforma.

---

## 1. Crear una cuenta

Crear una cuenta en Ollama.

---

## 2. Instalar Ollama

Descargar e instalar la aplicación de escritorio correspondiente al sistema operativo.

Verificar la instalación:

```bash
ollama --version
```

---

## 3. Seleccionar un modelo

Ingresar a la biblioteca de modelos de Ollama.

Por ejemplo:

```text
https://ollama.com/library/qwen3.5
```

Los modelos pueden estar disponibles para:

- ejecución local;
- descarga;
- ejecución mediante servicios de Ollama.

---

## 4. Copiar el comando para OpenCode

En la página del modelo se puede obtener el comando para ejecutarlo con OpenCode.

El formato general es:

```bash
ollama launch opencode --model <modelo>
```

Ejemplos:

```bash
ollama launch opencode --model qwen3.5
```

```bash
ollama launch opencode --model granite3.3:2b
```

```bash
ollama launch opencode --model hermes3:3b
```

```bash
ollama launch opencode --model qwen2.5-coder:3b
```

---

## 5. Ejecutar el modelo

Pegar el comando en la terminal.

Por ejemplo:

```bash
ollama launch opencode --model qwen2.5-coder:3b
```

Ollama iniciará OpenCode utilizando el modelo seleccionado.

---

## 6. Verificar modelos disponibles

Dentro de OpenCode ejecutar:

```text
/models
```

El modelo iniciado mediante Ollama debería aparecer en la lista de modelos disponibles.

---

# Consultar y cambiar modelos

Dentro de OpenCode se puede ejecutar:

```text
/models
```

Este comando muestra los modelos disponibles para los proveedores configurados.

Desde allí se puede seleccionar el modelo que se desea utilizar.

---

# Crear una nueva conversación

Dentro de OpenCode presionar:

```text
Ctrl + X
```

y después:

```text
N
```

La secuencia:

```text
Ctrl + X
N
```

permite crear una nueva conversación.

---

# Acceder a comandos con Ctrl + X

El shortcut:

```text
Ctrl + X
```

permite acceder a distintos comandos y acciones de OpenCode.

Por ejemplo:

```text
Ctrl + X
N
```

crea una nueva conversación.

Conviene revisar las opciones disponibles mediante este shortcut para conocer otras funciones de navegación y configuración.

---

# Flujo general

```text
Abrir terminal
      ↓
Ingresar al directorio de trabajo
      ↓
opencode
      ↓
/connect
      ↓
Seleccionar proveedor
      ↓
Configurar autenticación
      ↓
Seleccionar modelo
      ↓
Crear conversación
      ↓
Validar funcionamiento
```

---

# Resumen por proveedor

## OpenCode Zen

```text
/connect
   ↓
OpenCode Zen
   ↓
Crear cuenta
   ↓
Obtener API key
   ↓
Pegar API key
   ↓
Seleccionar modelo
```

## OpenAI

```text
/connect
   ↓
OpenAI
   ↓
ChatGPT Pro/Plus (browser)
   ↓
Autenticarse en navegador
   ↓
Seleccionar workspace
   ↓
Seleccionar modelo
```

## Ollama

```text
Instalar Ollama
   ↓
Seleccionar modelo
   ↓
ollama launch opencode --model <modelo>
   ↓
Abrir OpenCode
   ↓
/models
```

---

# Resultado

Después de configurar uno o varios proveedores:

- OpenCode puede utilizar diferentes proveedores de IA.
- Se pueden consultar los modelos disponibles mediante:

```text
/models
```

- Es posible cambiar de modelo según las necesidades del proyecto.
- OpenAI puede autenticarse utilizando ChatGPT Plus o Pro mediante navegador.
- OpenCode Zen puede configurarse mediante API key.
- Ollama permite integrar modelos locales o compatibles con su plataforma.
- Se pueden crear nuevas conversaciones mediante:

```text
Ctrl + X
N
```
