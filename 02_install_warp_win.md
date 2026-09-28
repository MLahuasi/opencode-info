# Configuración de Warp en Windows 10 Home

## 1. Instalación

La instalación mediante `winget` presentó un error de validación del hash:

```powershell
winget install Warp.Warp
```

```text
El hash del instalador no coincide
```

Por este motivo se descargó e instaló Warp manualmente desde su sitio oficial.

La ruta utilizada para la instalación fue:

```text
D:\Programas\Console\Warp
```

El ejecutable quedó en:

```text
D:\Programas\Console\Warp\warp.exe
```

Se puede comprobar con:

```powershell
Test-Path "D:\Programas\Console\Warp\warp.exe"
```

Resultado esperado:

```text
True
```

---

## 2. Problema de inicio

Warp se instalaba correctamente, pero al ejecutarlo:

```powershell
& "D:\Programas\Console\Warp\warp.exe"
```

el proceso terminaba y la interfaz no aparecía.

Los eventos de Windows mostraron que el problema estaba relacionado con el renderizado gráfico de Warp.

La solución que permitió iniciar Warp fue forzar el backend gráfico **DirectX 12**:

```powershell
$env:WGPU_BACKEND="dx12"
& "D:\Programas\Console\Warp\warp.exe"
```

Con esta configuración Warp pudo abrir correctamente.

> `WGPU_BACKEND` no se configuró globalmente en Windows para evitar afectar otras aplicaciones.

---

## 3. Crear un wrapper para Warp

Para no tener que definir `WGPU_BACKEND` manualmente cada vez, se creó:

```text
D:\Programas\Console\Warp\bin\warp.cmd
```

Contenido:

```bat
@echo off
set "WGPU_BACKEND=dx12"
start "" "D:\Programas\Console\Warp\warp.exe"
```

La estructura queda:

```text
D:\Programas\Console\Warp\
├── warp.exe
└── bin\
    ├── warp.cmd
    └── oz.cmd
```

Se puede probar directamente desde PowerShell:

```powershell
& "D:\Programas\Console\Warp\bin\warp.cmd"
```

El flujo de ejecución es:

```text
warp.cmd
   ↓
WGPU_BACKEND=dx12
   ↓
warp.exe
```

---

## 4. Configurar `warp` como comando de consola

El objetivo es que:

```powershell
warp
```

ejecute `warp.cmd` y no directamente `warp.exe`.

En el `PATH` de usuario debe mantenerse:

```text
D:\Programas\Console\Warp\bin
```

y evitar:

```text
D:\Programas\Console\Warp
```

porque esa carpeta contiene `warp.exe` y Windows podría ejecutarlo directamente sin aplicar `WGPU_BACKEND=dx12`.

### Modificar el PATH

```powershell
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")

$paths = $userPath -split ";" |
    Where-Object {
        $_ -and
        $_ -ne "D:\Programas\Console\Warp" -and
        $_ -ne "D:\Programas\Console\Warp\bin"
    }

$newPath = "D:\Programas\Console\Warp\bin;" + ($paths -join ";")

[Environment]::SetEnvironmentVariable(
    "Path",
    $newPath,
    "User"
)
```

Actualizar el `PATH` de la sesión actual:

```powershell
$env:Path =
    [Environment]::GetEnvironmentVariable("Path", "Machine") + ";" +
    [Environment]::GetEnvironmentVariable("Path", "User")
```

Verificar:

```powershell
$env:Path -split ";" |
    Where-Object { $_ -like "*Warp*" }
```

Resultado esperado:

```text
D:\Programas\Console\Warp\bin
```

Comprobar qué comando resolverá Windows:

```powershell
where.exe warp
```

Resultado esperado:

```text
D:\Programas\Console\Warp\bin\warp.cmd
```

A partir de ese momento:

```powershell
warp
```

debe ejecutar:

```text
warp
 ↓
warp.cmd
 ↓
WGPU_BACKEND=dx12
 ↓
warp.exe
```

---

## 5. Corregir el acceso directo del menú Inicio

El instalador creó inicialmente un acceso directo que apuntaba directamente a:

```text
D:\Programas\Console\Warp\warp.exe
```

Eso evitaba que se aplicara:

```text
WGPU_BACKEND=dx12
```

El acceso directo encontrado fue:

```text
C:\Users\Developer\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Warp.lnk
```

### Localizar accesos directos de Warp

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

---

## 6. Hacer que el menú Inicio utilice `warp.cmd`

Modificar el acceso directo:

```powershell
$shortcutPath =
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Warp.lnk"

$ws = New-Object -ComObject WScript.Shell
$shortcut = $ws.CreateShortcut($shortcutPath)

$shortcut.TargetPath =
    "C:\Windows\System32\cmd.exe"

$shortcut.Arguments =
    '/c ""D:\Programas\Console\Warp\bin\warp.cmd""'

$shortcut.WorkingDirectory =
    "D:\Programas\Console\Warp"

$shortcut.IconLocation =
    "D:\Programas\Console\Warp\warp.exe,0"

$shortcut.WindowStyle = 7

$shortcut.Save()
```

### Verificar el acceso directo

```powershell
$shortcut = $ws.CreateShortcut(
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Warp.lnk"
)

$shortcut.TargetPath
$shortcut.Arguments
$shortcut.WorkingDirectory
```

Resultado esperado:

```text
C:\Windows\System32\cmd.exe
/c ""D:\Programas\Console\Warp\bin\warp.cmd""
D:\Programas\Console\Warp
```

El icono sigue utilizando el recurso gráfico original de:

```text
D:\Programas\Console\Warp\warp.exe
```

---

## 7. Acceso directo del escritorio

Si se crea un acceso directo manual, no debe ejecutar directamente:

```text
D:\Programas\Console\Warp\warp.exe
```

Debe utilizar:

```text
C:\Windows\System32\cmd.exe
```

con los argumentos:

```text
/c ""D:\Programas\Console\Warp\bin\warp.cmd""
```

Como directorio de inicio:

```text
D:\Programas\Console\Warp
```

Y como icono puede utilizarse:

```text
D:\Programas\Console\Warp\warp.exe
```

---

## 8. Arquitectura final

Todos los mecanismos de inicio deben terminar utilizando el mismo wrapper:

```text
                   ┌─ Menú Inicio
                   │
                   ├─ Acceso directo
                   │
                   └─ PowerShell: warp
                           │
                           ▼
        D:\Programas\Console\Warp\bin\warp.cmd
                           │
                           ▼
                 WGPU_BACKEND=dx12
                           │
                           ▼
        D:\Programas\Console\Warp\warp.exe
```

Esto evita configurar `WGPU_BACKEND` globalmente y aplica DirectX 12 únicamente a Warp.

---

## 9. Comprobaciones rápidas

### Ejecutable

```powershell
Test-Path "D:\Programas\Console\Warp\warp.exe"
```

### Wrapper

```powershell
Test-Path "D:\Programas\Console\Warp\bin\warp.cmd"
```

### PATH

```powershell
$env:Path -split ";" |
    Where-Object { $_ -like "*Warp*" }
```

### Resolución del comando

```powershell
where.exe warp
```

Resultado deseado:

```text
D:\Programas\Console\Warp\bin\warp.cmd
```

### Ejecutar Warp

```powershell
warp
```

## Configuración utilizada

```text
Sistema operativo : Windows 10 Home
Warp               : D:\Programas\Console\Warp
Ejecutable         : D:\Programas\Console\Warp\warp.exe
Wrapper            : D:\Programas\Console\Warp\bin\warp.cmd
Backend gráfico    : DirectX 12
Variable           : WGPU_BACKEND=dx12
```
