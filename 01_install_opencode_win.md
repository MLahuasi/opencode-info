# Instalar OpenCode en Windows 10 Home

Esta guía instala OpenCode globalmente con npm y lo inicia desde la carpeta de un proyecto.

## Requisitos

Verifica que Node.js y npm estén disponibles en PowerShell:

```powershell
node --version
npm --version
```

Si alguno de los comandos no se reconoce, instala Node.js antes de continuar.

## Instalar OpenCode

Ejecuta en PowerShell:

```powershell
npm install -g --allow-scripts=opencode-ai opencode-ai
```

La opción `--allow-scripts=opencode-ai` permite que npm ejecute el script de instalación de ese paquete. Sin esa autorización, npm puede mostrar una advertencia indicando que bloqueó el `postinstall`.

## Verificar la instalación

Comprueba que PowerShell encuentre OpenCode:

```powershell
opencode --version
```

En la instalación documentada se obtuvo `1.18.25`. La versión puede ser distinta si el paquete se actualiza.

## Iniciar OpenCode en un proyecto

Cambia a la carpeta del proyecto y ejecuta OpenCode:

```powershell
cd D:\ruta\del\proyecto
opencode
```

OpenCode utilizará la carpeta actual como workspace.

## Nota sobre el instalador de Unix

El comando `curl -fsSL https://opencode.ai/install | bash` está pensado para entornos con `bash`. No lo ejecutes directamente en PowerShell: allí `curl` puede ser un alias de `Invoke-WebRequest`, que no admite las mismas opciones. Para esta guía de Windows se utiliza npm.
