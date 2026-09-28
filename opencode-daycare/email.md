## Configurar Email (Instalar dependencia externa)

- En modo `Plan`

```text
/spec
instalar https://github.com/MLahuasi/jmlq-mailer basado es esta implementación https://github.com/MLahuasi/jmlq-ecosystem/tree/main/src/infrastructure/adapters/jmlq/mailer. Se debe crear una capa Infrastructure/adapters/jmlq/mailer. Se debe crear una plantilla similar a la siguiente https://github.com/MLahuasi/jmlq-ecosystem/blob/main/src/templates/verify-email.html pero con el estilo del branding de OpenDayCare (este correo enviará el email con el link cuando se relaciona a un padre a un niño) dentro de un directorio de templates.
```

- Una vez analizado el plan, en modo `Build`:

```text
crea spec
```

- Analizar el spec creado [07-parent-invitation-mailer.md](../../open-daycare/specs/07-parent-invitation-mailer.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/07-parent-invitation-mailer.md
```

- Cuando crea la rama `spec-07-parent-invitation-mailer` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-07-parent-invitation-mailer"
```

- También se creó el plan:

```md
### Implementation Plan

1.  Instalar @jmlq/mailer y actualizar los archivos de npm.
2.  Permitir .env.template en .gitignore.
3.  Crear .env.template.
4.  Crear directorios y barrels de Infrastructure.
5.  Crear la configuración diferida del mailer.
6.  Crear el singleton mediante la API pública del paquete.
7.  Crear tipos, validaciones, escaping y formato de fecha.
8.  Implementar sendParentInvitationEmail.
9.  Crear la plantilla HTML.
10. Exponer únicamente el contrato público necesario.
11. Documentar la configuración en README.md.
12. Actualizar el diagrama feature-first.
13. Verificar el renderizado con datos controlados.
14. Verificar errores de configuración sin contactar SMTP.
15. Revisar arquitectura, seguridad, imports, JSDoc y estilos.
16. Ejecutar ESLint, TypeScript, build y git diff --check.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-07-parent-invitation-mailer - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-07-parent-invitation-mailer
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada `spec-07-parent-invitation-mailer`
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-07-parent-invitation-mailer
```
