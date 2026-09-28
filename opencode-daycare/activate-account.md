### Activar Cuenta

- En modo `Plan`

```
/spec
Implementar la página con la ruta `auth/link-parent`, basada en la plantilla `@references/screens/vincular-padre.dc.html`, invocada desde `kids/[slug]` al hacer clic en "Vincular otro padre".
`auth/link-parent` recibe el `id` del niño y carga la información relacionada con `Invitation`, `Credential` y `Person`.
En `auth/link-parent` se debe generar un "Código de Invitación". Además, se debe implementar la funcionalidad para crear el código y definir su tiempo de vencimiento (Debe ser un tool).
Al hacer clic en "Enviar invitación", se debe enviar un correo utilizando la implementación definida en el spec `07-parent-invitation-mailer.md`.
Cuando el padre haga clic en el enlace del correo, se debe cargar la página `auth/activate-account` con la información correspondiente.
Una vez activada la cuenta, el usuario puede ingresar al sistema desde `auth/login`. Al hacer clic en "Iniciar Sesión", con correo y contraseña correctos, se debe redirigir a `/home`.

```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [09-account-activation-session-and-login.md](../../open-daycare/specs/09-account-activation-session-and-login.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/09-account-activation-session-and-login.md
```

- Cuando crea la rama `spec-09-account-activation-session-and-login` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-09-account-activation-session-and-login"
```

- También se creó el plan:

```md
## Implementation Plan

1.  Instalar dependencias y actualizar configuración.
2.  Normalizar Invitation, fixtures y servicios.
3.  Crear configuración estable de NextAuth v4 y Route Handler.
4.  Añadir callbacks JWT/sesión con personId y role.
5.  Crear transacción JSON con lock, snapshots y rollback.
6.  Implementar Server Action de activación.
7.  Actualizar pantalla de activación.
8.  Crear credencial bcrypt de Caro.
9.  Implementar autorización server-side.
10. Conectar Login.
11. Conectar logout y mensaje de activación.
12. Mostrar estado seguro en /home.
13. Verificar activación, persistencia, rollback, autenticación y autorización.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-09-account-activation-session-and-login - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-09-account-activation-session-and-login
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-09-account-activation-session-and-login
```

---

- Analizar el spec creado [10-parent-linking-and-invitation.md](../../open-daycare/specs/10-parent-linking-and-invitation.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/10-parent-linking-and-invitation.md
```

- Cuando crea la rama `spec-10-parent-linking-and-invitation` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-10-parent-linking-and-invitation"
```

- También se creó el plan:

```md
## Plan de implementación

1.  Crear el servicio server-only que carga el niño por ID.
2.  Crear la utilidad de código y vencimiento.
3.  Crear el schema del formulario.
4.  Crear página y formulario responsive.
5.  Enlazar el botón del perfil.
6.  Crear la Server Action y validar en servidor.
7.  Rechazar emails existentes.
8.  Crear Person parent/pending.
9.  Crear Invitation.
10. Persistir bajo lock antes del mailer.
11. Construir enlace con APP_URL y enviar email.
12. Actualizar sentAt tras envío exitoso.
13. Conservar registros si falla SMTP.
14. Reutilizar invitaciones vigentes y rotar vencidas.
15. Redirigir con invitation=sent y mostrar banner.
16. Verificar con SMTP de prueba sin credenciales en el repositorio.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-10-parent-linking-and-invitation - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-10-parent-linking-and-invitation
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-10-parent-linking-and-invitation
```

---

- Analizar el spec creado [11-family-home-feed.md](../../open-daycare/specs/11-family-home-feed.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/11-family-home-feed.md
```

- Cuando crea la rama `spec-11-family-home-feed` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-11-family-home-feed"
```

- También se creó el plan:

```md
## Implementation Plan

1.  Añadir kidId y roomId a FeedPost y migrar feed.json.
2.  Validar que cada publicación tenga exactamente un destino.
3.  Crear servicios server-only para sesión, personas, relaciones, niños y salas.
4.  Crear la proyección del feed familiar.
5.  Crear el Home familiar reutilizando las tarjetas existentes.
6.  Crear el header familiar.
7.  Seleccionar composición de personal o familiar según el rol.
8.  Mostrar estado vacío para cuentas sin relaciones.
9.  Verificar feed combinado y anuncios sin duplicados.
10. Verificar aislamiento de publicaciones y salas ajenas.
11. Verificar Home, logout, responsive y accesibilidad con Playwright.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare -  spec-11-family-home-feed - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin  spec-11-family-home-feed
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d  spec-11-family-home-feed
```
