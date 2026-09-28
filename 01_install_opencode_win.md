# Instalar OpenCode en Windows 10 Home

Esta guía instala OpenCode globalmente con npm y lo inicia desde la carpeta de un proyecto.

## 1. Comprobar los requisitos

Verifica que Node.js y npm estén disponibles en PowerShell:

```powershell
node --version
npm --version
```

Si alguno de los comandos no se reconoce, instala Node.js antes de continuar.

## 2. Instalar OpenCode

Ejecuta en PowerShell:

```powershell
npm install -g --allow-scripts=opencode-ai opencode-ai
```

La opción `--allow-scripts=opencode-ai` autoriza a npm a ejecutar el script `postinstall` de OpenCode. Sin esa autorización, npm puede bloquear el script y mostrar una advertencia.

## 3. Verificar la instalación

Comprueba que PowerShell encuentre OpenCode:

```powershell
opencode --version
```

En la instalación documentada se obtuvo `1.18.25`. La versión puede ser distinta si el paquete se actualiza.

## 4. Iniciar OpenCode en un proyecto

Cambia `D:\ruta\del\proyecto` por la ruta de la carpeta que quieres usar como workspace. Luego ejecuta:

```powershell
cd D:\ruta\del\proyecto
opencode
```

OpenCode utilizará la carpeta actual como workspace.

## Nota: instalador para Unix

El comando `curl -fsSL https://opencode.ai/install | bash` está pensado para entornos con `bash`. No lo ejecutes directamente en PowerShell: allí `curl` puede ser un alias de `Invoke-WebRequest`, que no admite las mismas opciones. Para esta guía de Windows se utiliza npm.
