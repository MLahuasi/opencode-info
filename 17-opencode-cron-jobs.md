# Ejecutar CRON Jobs con OpenCode

Este tema presenta distintas formas de ejecutar tareas programadas que utilizan OpenCode o uno de sus agentes.

OpenCode puede ejecutarse sin abrir la TUI mediante `opencode run`, una modalidad pensada para scripts, CI y otros procesos automatizados.

## Plan objetivo: auditoría semanal de seguridad

El objetivo es ejecutar todos los viernes a las 18:00 el agente `db-security-auditor` para auditar el proyecto Supabase remoto de test en modo estrictamente de solo lectura.

La sintaxis correcta de OpenCode V2 es:

```bash
opencode run \
  --standalone \
  --agent db-security-auditor \
  --auto \
  "Audita el Supabase remoto de test autorizado en modo estrictamente de solo lectura. Usa únicamente CLI remoto con --read-only. No uses MCP, Docker ni servicios locales. No modifiques archivos ni la base de datos. No muestres secretos, hashes, tokens ni filas sensibles."
```

`opencode -run @db-security-auditor` no es la sintaxis válida para esta automatización. `--auto` evita que un proceso desatendido quede esperando confirmaciones; debe combinarse con permisos restrictivos y con el agente configurado para no modificar archivos ni bases de datos.

En los ejemplos locales se usa la hora del equipo. En GitHub Actions se declara una zona horaria IANA y en Supabase Cron se usa UTC/GMT.

> **Contexto de los ejemplos:** los horarios, rutas, nombres de agentes, comandos y secretos son ejemplos. Adáptalos al sistema operativo, al proyecto y a la política de seguridad de tu entorno.

## 1. Elegir el mecanismo de ejecución

La opción adecuada depende del lugar donde se ejecutará el agente:

| Opción                      | Entorno recomendado             | Uso habitual                                                |
| --------------------------- | ------------------------------- | ----------------------------------------------------------- |
| Cron del sistema            | Linux y macOS                   | Tareas locales o servidores Unix.                           |
| Task Scheduler              | Windows                         | Tareas locales administradas desde la interfaz gráfica.     |
| PowerShell + Task Scheduler | Windows                         | Tareas versionadas como scripts.                            |
| `launchd`                   | macOS                           | Tareas nativas de macOS.                                    |
| Supabase Cron (`pg_cron`)   | Supabase/PostgreSQL             | Tareas relacionadas con datos y funciones de base de datos. |
| GitHub Actions              | Repositorios alojados en GitHub | Automatización remota y tareas periódicas del repositorio.  |

Antes de programar la tarea, ejecuta el comando manualmente y confirma que el proyecto, el proveedor, las credenciales y el agente funcionan correctamente.

## 2. Cron del sistema en Linux y macOS

El cron del sistema ejecuta comandos según una expresión de cinco campos:

```text
minuto hora día-del-mes mes día-de-la-semana
```

Ejemplo para ejecutar la auditoría todos los viernes a las 18:00, en la zona horaria local del equipo:

```cron
0 18 * * 5 /ruta/scripts/run-db-security-audit.sh >> /ruta/logs/db-security-audit.log 2>&1
```

Wrapper recomendado (`/ruta/scripts/run-db-security-audit.sh`):

```bash
#!/usr/bin/env bash
set -Eeuo pipefail
umask 077

cd /ruta/al/proyecto
OPENCODE="/ruta/absoluta/al/opencode"

"$OPENCODE" run \
  --standalone \
  --agent db-security-auditor \
  --auto \
  "Audita el Supabase remoto de test autorizado en modo estrictamente de solo lectura. Usa únicamente CLI remoto con --read-only. No uses MCP, Docker ni servicios locales. No modifiques archivos ni la base de datos. No muestres secretos, hashes, tokens ni filas sensibles."
```

Edita el crontab del usuario con:

```bash
crontab -e
```

El proceso debe tener acceso al proyecto, al ejecutable de OpenCode y a las variables de entorno necesarias. Utiliza rutas absolutas y registra la salida para poder diagnosticar errores.

## 3. Task Scheduler en Windows

Task Scheduler permite crear una tarea desde la interfaz gráfica de Windows:

1. Abre **Task Scheduler**.
2. Selecciona **Create Task**.
3. Define el nombre y la cuenta que ejecutará la tarea.
4. En **Triggers**, selecciona frecuencia semanal, viernes, 18:00.
5. En **Actions**, selecciona `pwsh.exe` y usa `-NoLogo -NoProfile -NonInteractive -File "C:\Ops\run-db-security-audit.ps1"`.
6. Configura como directorio de inicio la raíz del proyecto.
7. En **Settings**, activa *Run task as soon as possible after a scheduled start is missed* y selecciona *Do not start a new instance*.
8. Ejecuta la tarea manualmente para verificarla.

Esta alternativa es adecuada cuando se quiere administrar la programación mediante una interfaz gráfica.

## 4. PowerShell y una tarea programada

Guardar la lógica en un script facilita versionarla y probarla fuera del programador de tareas.

Ejemplo de script `C:\Ops\run-db-security-audit.ps1`:

```powershell
$ErrorActionPreference = "Stop"

Set-Location "C:\ruta\al\proyecto"
$OpenCode = "C:\Program Files\nodejs\opencode.ps1"
$Prompt = "Audita el Supabase remoto de test autorizado en modo estrictamente de solo lectura. Usa únicamente CLI remoto con --read-only. No uses MCP, Docker ni servicios locales. No modifiques archivos ni la base de datos. No muestres secretos, hashes, tokens ni filas sensibles."

& $OpenCode run --standalone --agent db-security-auditor --auto $Prompt *>> "C:\ruta\logs\db-security-audit.log"
exit $LASTEXITCODE
```

Puedes crear una tarea programada desde PowerShell:

```powershell
$action = New-ScheduledTaskAction `
  -Execute "powershell.exe" `
  -Argument "-NoProfile -File C:\ruta\scripts\opencode-cron.ps1"

$trigger = New-ScheduledTaskTrigger -Weekly -WeeksInterval 1 -DaysOfWeek Friday -At 6:00PM

Register-ScheduledTask `
  -TaskName "OpenDaycare weekly DB security audit" `
  -Action $action `
  -Trigger $trigger `
  -Description "Auditoría semanal read-only de Supabase test con OpenCode"
```

Los nombres, horarios y rutas anteriores son ejemplos. Prueba el script con la misma cuenta que utilizará la tarea para detectar diferencias de permisos o de entorno.

## 5. `launchd` en macOS — recomendado nativo

`launchd` es el mecanismo nativo de macOS para ejecutar tareas administradas por el sistema. Una tarea de usuario puede definirse en `~/Library/LaunchAgents/`.

Ejemplo de archivo `com.opendaycare.db-security-audit.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.opendaycare.db-security-audit</string>
  <key>ProgramArguments</key>
  <array>
    <string>/ruta/absoluta/al/opencode</string>
    <string>run</string>
    <string>--standalone</string>
    <string>--agent</string>
    <string>db-security-auditor</string>
    <string>--auto</string>
    <string>Audita el Supabase remoto de test autorizado en modo estrictamente de solo lectura. Usa únicamente CLI remoto con --read-only. No uses MCP, Docker ni servicios locales. No modifiques archivos ni la base de datos. No muestres secretos, hashes, tokens ni filas sensibles.</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict>
    <key>Weekday</key>
    <integer>5</integer>
    <key>Hour</key>
    <integer>18</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>
  <key>StandardOutPath</key>
  <string>/ruta/logs/db-security-audit.log</string>
  <key>StandardErrorPath</key>
  <string>/ruta/logs/db-security-audit-error.log</string>
</dict>
</plist>
```

Carga la tarea con:

```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.opendaycare.db-security-audit.plist
```

Para descargarla:

```bash
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.opendaycare.db-security-audit.plist
```

## 6. Supabase Cron (`pg_cron`) — solo como puente HTTP

`pg_cron` no puede ejecutar directamente el binario local `opencode`. Puede ejecutar SQL, funciones PostgreSQL o solicitudes HTTP mediante `pg_net`. Para este plan debe utilizarse únicamente como puente hacia GitHub Actions u otro runner seguro.

El horario `0 18 * * 5` de Supabase Cron es viernes a las 18:00 UTC/GMT, no necesariamente a las 18:00 locales.

Ejemplo conceptual:

```sql
select cron.schedule(
  'weekly-db-security-auditor',
  '0 18 * * 5',
  $$
  select net.http_post(
    url := 'https://api.github.com/repos/ORGANIZACION/REPOSITORIO/actions/workflows/db-security-audit.yml/dispatches',
    headers := jsonb_build_object(
      'Authorization', 'Bearer ' || (select decrypted_secret from vault.decrypted_secrets where name = 'github_actions_dispatch_token'),
      'Accept', 'application/vnd.github+json',
      'X-GitHub-Api-Version', '2022-11-28',
      'User-Agent', 'supabase-cron'
    ),
    body := jsonb_build_object('ref', 'main')
  );
  $$
);
```

El endpoint debe validar la autenticación, registrar la ejecución y evitar que una solicitud no autorizada invoque un agente. No guardes tokens directamente en el repositorio ni expongas secretos en el SQL versionado.

## 7. GitHub Actions — recomendado para un repositorio en GitHub

GitHub Actions permite ejecutar tareas programadas cuando el repositorio está alojado en GitHub. El workflow debe declarar el evento `schedule` y configurar los secretos necesarios en GitHub.

Ejemplo de `.github/workflows/db-security-audit.yml`:

```yaml
name: Weekly DB security audit

on:
  schedule:
    - cron: "0 18 * * 5"
      timezone: "Europe/Madrid"
  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: weekly-db-security-audit
  cancel-in-progress: false

jobs:
  run-opencode:
    runs-on: ubuntu-latest
    timeout-minutes: 60
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install OpenCode
        run: npm install --global @opencode/cli@2.1.2

      - name: Run scheduled task
        env:
          SECURITY_AUDITOR_DB_URL: ${{ secrets.SECURITY_AUDITOR_DB_URL }}
          SECURITY_AUDITOR_DB_PASSWORD: ${{ secrets.SECURITY_AUDITOR_DB_PASSWORD }}
          OPENROUTER_API_KEY: ${{ secrets.OPENROUTER_API_KEY }}
        run: |
          opencode run \
            --standalone \
            --agent db-security-auditor \
            --auto \
            "Audita el Supabase remoto de test autorizado en modo estrictamente de solo lectura. Usa únicamente CLI remoto con --read-only. No uses MCP, Docker ni servicios locales. No modifiques archivos ni la base de datos. No muestres secretos, hashes, tokens ni filas sensibles."
```

GitHub Actions puede sufrir retrasos cuando muchas tareas coinciden en el mismo minuto. `workflow_dispatch` permite probar el workflow manualmente. El workflow debe existir en la rama por defecto y el checkout normal no incluye referencias externas como `../info/opencode-daycare/references/schemas`; si son necesarias, deben incorporarse explícitamente al entorno de CI.

## 8. Seguridad y operación

- Utiliza una cuenta con los permisos mínimos necesarios.
- Guarda las API keys y tokens en variables de entorno o secretos del sistema.
- No incluyas secretos en prompts, scripts versionados, logs ni archivos de configuración públicos.
- Define un timeout y evita ejecuciones concurrentes cuando la tarea no sea idempotente.
- Registra inicio, finalización, duración, salida y errores.
- Revisa los cambios que un agente pueda realizar antes de publicarlos.
- Prueba primero el comando manualmente y después desde el scheduler.
- Usa un mecanismo de bloqueo si una ejecución no debe solaparse con otra.
- El agente `db-security-auditor` debe utilizar exclusivamente una conexión autorizada de test y read-only; nunca producción.
- Configura `SECURITY_AUDITOR_DB_URL` y `SECURITY_AUDITOR_DB_PASSWORD` como secretos del scheduler o de GitHub, nunca en el repositorio.
- No uses `--auto` sin revisar antes los permisos efectivos del agente y del proyecto.
- Rota cualquier token que haya quedado versionado accidentalmente en `opencode.json` y sustitúyelo por una referencia a entorno.

## 9. Recomendación de implementación

1. Probar manualmente `opencode run --agent db-security-auditor` con una conexión read-only al Supabase remoto de test.
2. En Windows local, usar **Task Scheduler con GUI** y el script PowerShell.
3. En macOS, usar **launchd**; en Linux, cron con wrapper y rutas absolutas.
4. Si el repositorio está en GitHub, preferir **GitHub Actions**, con `workflow_dispatch`, `timezone`, `concurrency`, `timeout` y secretos configurados.
5. Usar Supabase Cron únicamente para disparar por HTTP el workflow o un endpoint protegido; no para ejecutar OpenCode dentro de PostgreSQL.

---

[← Anterior](16-opencode-subagents.md) | [Índice principal](README.md)
