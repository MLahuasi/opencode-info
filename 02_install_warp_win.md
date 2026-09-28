# Instalar y configurar Warp en Windows 10 Home

Esta guía documenta una instalación de Warp en `D:\Programas\Console\Warp`. En ese equipo, Warp no se abrió correctamente con su configuración predeterminada; definir `WGPU_BACKEND=dx12` permitió iniciar la interfaz. Las rutas mostradas corresponden a ese entorno y deben ajustarse si Warp se instala en otra ubicación.

## 1. Instalar Warp

Se intentó instalar Warp con `winget`:

```powershell
winget install Warp.Warp
```

La instalación falló con un error de validación del hash:

```text
El hash del instalador no coincide
```

Se descargó e instaló Warp manualmente desde su sitio oficial. En este entorno, el ejecutable quedó en:

```text
D:\Programas\Console\Warp\warp.exe
```

Comprueba que exista:

```powershell
Test-Path "D:\Programas\Console\Warp\warp.exe"
```

El resultado esperado es `True`.

## 2. Crear un wrapper para iniciar Warp

Al ejecutar directamente `warp.exe`, el proceso terminaba sin mostrar la interfaz. Forzar el backend gráfico DirectX 12 permitió abrir Warp:

```powershell
$env:WGPU_BACKEND = "dx12"
& "D:\Programas\Console\Warp\warp.exe"
```

Para aplicar esta variable solo a Warp y no globalmente a Windows, crea el archivo `D:\Programas\Console\Warp\bin\warp.cmd` con este contenido:

```bat
@echo off
set "WGPU_BACKEND=dx12"
start "" "D:\Programas\Console\Warp\warp.exe"
```

El wrapper establece el backend y luego inicia el ejecutable. Comprueba que el archivo exista y pruébalo:

```powershell
Test-Path "D:\Programas\Console\Warp\bin\warp.cmd"
& "D:\Programas\Console\Warp\bin\warp.cmd"
```

## 3. Configurar `warp` como comando de consola

Añade `D:\Programas\Console\Warp\bin` al `PATH` de usuario. No añadas `D:\Programas\Console\Warp`: esa carpeta contiene `warp.exe`, que se podría ejecutar sin pasar por el wrapper.

El siguiente bloque quita ambas rutas de cualquier posición y agrega `bin` al inicio del `PATH` de usuario:

```powershell
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")

$paths = $userPath -split ";" |
    Where-Object {
        $_ -and
        $_ -ne "D:\Programas\Console\Warp" -and
        $_ -ne "D:\Programas\Console\Warp\bin"
    }

$newPath = "D:\Programas\Console\Warp\bin;" + ($paths -join ";")

[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
```

Actualiza el `PATH` de la sesión actual para poder probarlo sin abrir otra ventana de PowerShell:

```powershell
$env:Path =
    [Environment]::GetEnvironmentVariable("Path", "Machine") + ";" +
    [Environment]::GetEnvironmentVariable("Path", "User")
```

Comprueba que Windows resuelva `warp` al wrapper:

```powershell
where.exe warp
```

El resultado esperado es:

```text
D:\Programas\Console\Warp\bin\warp.cmd
```

Ahora puedes iniciar Warp desde PowerShell con:

```powershell
warp
```

## 4. Configurar los accesos directos

El acceso directo del menú Inicio apuntaba directamente a `warp.exe`, por lo que no aplicaba la variable del wrapper. Para localizar accesos directos de Warp en los menús Inicio del usuario y del sistema:

```powershell
$ws = New-Object -ComObject WScript.Shell

@(
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs",
    "$env:ProgramData\Microsoft\Windows\Start Menu\Programs"
) |
ForEach-Object {
    Get-ChildItem $_ -Filter "*.lnk" -Recurse -ErrorAction SilentlyContinue
} |
ForEach-Object {
    $shortcut = $ws.CreateShortcut($_.FullName)

    if ($shortcut.TargetPath -like "*Warp*") {
        [PSCustomObject]@{
            Shortcut  = $_.FullName
            Target    = $shortcut.TargetPath
            Arguments = $shortcut.Arguments
        }
    }
}
```

En este entorno, el acceso directo encontrado fue `C:\Users\Developer\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Warp.lnk`. Actualízalo para que inicie el wrapper mediante `cmd.exe`:

```powershell
$shortcutPath =
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Warp.lnk"

$ws = New-Object -ComObject WScript.Shell
$shortcut = $ws.CreateShortcut($shortcutPath)

$shortcut.TargetPath = "C:\Windows\System32\cmd.exe"
$shortcut.Arguments = '/c ""D:\Programas\Console\Warp\bin\warp.cmd""'
$shortcut.WorkingDirectory = "D:\Programas\Console\Warp"
$shortcut.IconLocation = "D:\Programas\Console\Warp\warp.exe,0"
$shortcut.WindowStyle = 7
$shortcut.Save()
```

Si creas un acceso directo de escritorio, usa la misma configuración: destino `C:\Windows\System32\cmd.exe`, argumentos `/c ""D:\Programas\Console\Warp\bin\warp.cmd""`, directorio de inicio `D:\Programas\Console\Warp` e icono `D:\Programas\Console\Warp\warp.exe`.

Verifica la configuración del acceso directo del menú Inicio:

```powershell
$shortcut = $ws.CreateShortcut(
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Warp.lnk"
)

$shortcut.TargetPath
$shortcut.Arguments
$shortcut.WorkingDirectory
```

Debe mostrar `cmd.exe` como destino, `warp.cmd` como argumento y la carpeta de Warp como directorio de inicio.

## Comprobación final

Confirma que el comando de consola use el wrapper y luego inicia Warp:

```powershell
where.exe warp
warp
```

El flujo de inicio queda centralizado en el wrapper:

```text
PowerShell / menú Inicio / acceso directo
                  ↓
          bin\warp.cmd
                  ↓
       WGPU_BACKEND=dx12
                  ↓
             warp.exe
```
