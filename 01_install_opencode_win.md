# Instalar OpenCode en Windows 10 Home

## Requisitos

Antes de instalar OpenCode, verificar que Node.js y npm estén disponibles:

```powershell
node --version
npm --version
```

## 1. Instalar OpenCode con npm

Abrir PowerShell y ejecutar:

```powershell
npm i -g opencode-ai
```

Durante la instalación puede aparecer una advertencia indicando que npm bloqueó el script `postinstall`:

```text
npm warn install-scripts 1 package had install scripts blocked because they are not covered by allowScripts
npm warn install-scripts opencode-ai (postinstall: node ./postinstall.mjs)
```

## 2. Autorizar el script de instalación

Ejecutar nuevamente la instalación permitiendo específicamente los scripts de `opencode-ai`:

```powershell
npm install -g --allow-scripts=opencode-ai opencode-ai
```

Esto permite que OpenCode complete correctamente su proceso de instalación.

## 3. Verificar la instalación

Ejecutar:

```powershell
opencode --version
```

En esta instalación se obtuvo:

```text
1.18.25
```

Esto confirma que OpenCode está instalado y disponible desde PowerShell.

## 4. Ejecutar OpenCode

Ubicarse en la carpeta del proyecto donde se desea trabajar:

```powershell
cd D:\ruta\del\proyecto
```

Ejecutar:

```powershell
opencode
```

OpenCode iniciará utilizando la carpeta actual como workspace.

## Nota sobre `curl`

El siguiente comando no debe ejecutarse directamente en PowerShell:

```bash
curl -fsSL https://opencode.ai/install | bash
```

En Windows PowerShell, `curl` puede resolverse como `Invoke-WebRequest`, que no reconoce parámetros como:

```text
-fsSL
```

Por este motivo, en Windows 10 Home se utilizó la instalación mediante npm:

```powershell
npm install -g --allow-scripts=opencode-ai opencode-ai
```
