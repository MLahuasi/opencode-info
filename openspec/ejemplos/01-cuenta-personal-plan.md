# Ejemplo: cuenta de personal

Este documento conserva el flujo de planificación utilizado para definir la página **Mi cuenta** del área de personal. El ejemplo todavía no incluye implementación: registra la exploración, las decisiones y la creación inicial del cambio OpenSpec.

## 1. Exploración con `@openspec-explore`

### Prompt inicial

```text
@openspec-explore se debe crear la pagina que es invocada desde "Mi Cuenta" en el area de personal.
La pagina cuenta con una seccion informativa que tiene un boton "Editar" que permite editar la
informacion de la persona, una seccion Notificaciones que permite habilitar notificaciones de
"Nuevos comentarios de familias" y "Recordatorio de resumen", una seccion que contiene
"Cambiar contrasena" y "Ayuda y soporte", finalmente un boton "Cerrar sesion".
```

### Hallazgos iniciales

OpenSpec identificó que ya existe un prototipo en `references/screens/mi-cuenta.dc.html` y que la nueva ruta probablemente sería `/account`, enlazada desde `src/presentation/navigation/staff-navigation.ts`.

También encontró:

- `StaffSidebar` ya admite marcar `account` como sección activa.
- “Mi cuenta” aparece en la navegación, pero todavía no tiene `href`.
- PostgreSQL ya contiene nombre, correo, teléfono y cargo de la persona.
- El cambio de contraseña y el cierre de sesión ya tienen comportamiento implementado.
- La cuenta familiar ya dispone de edición de nombre y teléfono, cambio de contraseña mediante modal y cierre de sesión.
- El diseño de referencia muestra nombre, correo, cargo y sala.

### Decisiones propuestas durante la exploración

- Ruta: `/account`, dentro de `src/app/(staff)/account/`.
- Acceso: cualquier sesión con capacidad de personal.
- Editables: nombre y teléfono.
- Informativos: correo, cargo y salas asignadas.
- Contraseña: reutilizar el caso de uso de autenticación existente.
- Cierre de sesión: reutilizar el componente actual.
- Ayuda y soporte: requiere una decisión de alcance.

OpenSpec detectó inicialmente que no existía una tabla de preferencias de notificaciones y preguntó si los interruptores debían persistirse.

## 2. Decisiones confirmadas

### Notificaciones

```text
Los dos interruptores de notificaciones por el momento no realizan ninguna acción,
pero "Editar" y "Cerrar sesión" realizan las acciones respectivas.
```

Decisión:

- Los interruptores son solamente presentacionales.
- Se muestran activados visualmente, pero deshabilitados.
- No se crea una tabla ni se añade persistencia.

### Cambio de contraseña

```text
Cambiar contraseña sí actualiza, es similar a como se maneja el cambio de contraseña
en "Mi cuenta" en el perfil familiar.
```

Decisión:

- Formulario modal.
- Nueva contraseña y confirmación.
- Mínimo de ocho caracteres.
- Validación de coincidencia.
- Conservación de la sesión activa.
- Mensajes de error y confirmación.
- Reutilización del caso de uso de autenticación existente.

### Edición del perfil

```text
Sí.
```

Decisión:

- Editables: nombre y teléfono.
- Solo lectura: correo, cargo y salas asignadas.
- Persistencia en PostgreSQL con aislamiento por guardería.
- Actualización de la página y de los datos mostrados en la navegación lateral.

### Ayuda y soporte

```text
Sí.
```

Decisión:

- Visible, pero deshabilitada.
- Sin navegación ni acción hasta definir un destino de soporte.

### Ruta y hora del resumen

```text
Sí.

Incluye hora fija.
```

Decisión:

- Ruta confirmada: `/account`.
- Accesible desde la navegación lateral y móvil del área de personal.
- La sección `account` aparece activa.
- El texto se muestra como **Recordatorio de resumen (17:00)**.

## 3. Alcance acordado

### Contenido

1. **Información personal**
   - Nombre.
   - Correo.
   - Cargo y salas asignadas.
   - Botón **Editar**.
2. **Edición**
   - Nombre y teléfono editables.
   - Correo, cargo y salas informativos.
   - Validación equivalente a la cuenta familiar.
   - Persistencia tenant-scoped en PostgreSQL.
3. **Notificaciones**
   - Nuevos comentarios de familias.
   - Recordatorio de resumen (17:00).
   - Interruptores visualmente activados y deshabilitados.
   - Sin persistencia ni acciones.
4. **Seguridad y soporte**
   - Cambio de contraseña funcional.
   - Ayuda y soporte visible y deshabilitada.
5. **Sesión**
   - Cierre de sesión funcional y redirección al login.

### Enfoque técnico acordado

```text
/account
   |
   +-- requireStaffSession
   +-- consulta tenant-scoped del perfil
   +-- StaffSidebar(activeSection="account")
   |
   +-- Editar
   |     +-- validación
   |     +-- Server Action
   |     +-- actualización de nombre y teléfono
   |
   +-- Cambiar contraseña
   |     +-- caso de uso existente de Auth
   |
   +-- Cerrar sesión
         +-- LogoutButton existente
```

No se requiere migración de base de datos: el nombre, el teléfono y las credenciales ya tienen persistencia; las notificaciones son presentacionales.

## 4. Verificación prevista

- Autorización del área de personal.
- Aislamiento por guardería.
- Edición válida e inválida del perfil.
- Cambio de contraseña.
- Cierre de sesión.
- Navegación activa en escritorio y móvil.
- Controles deshabilitados sin falsa interacción.
- Accesibilidad y diseño responsive.
- Lint, TypeScript y build.

## 5. Creación del cambio OpenSpec

Cuando la exploración terminó, se creó el cambio desde el chat de OpenCode:

```text
@openspec-new-change
```

Resultado:

```text
Cambio creado
- Nombre: add-staff-account-page
- Ubicación: openspec/changes/add-staff-account-page/
- Workflow: spec-driven
- Estado: 0/4 artefactos completados
```

Secuencia indicada por OpenSpec:

1. `proposal.md` — listo.
2. `specs/**/*.md` — bloqueado por la propuesta.
3. `design.md` — bloqueado por la propuesta.
4. `tasks.md` — bloqueado por las Specs y el diseño.

## 6. Creación de la propuesta

Después se solicitó continuar:

```text
Continua
```

OpenSpec creó:

```text
openspec/changes/add-staff-account-page/proposal.md
```

Y reportó:

- `proposal.md`: completado.
- Especificación `staff-account`: lista para redactar.
- `design.md`: listo para redactar.
- `tasks.md`: bloqueado hasta completar la especificación y el diseño.

La propuesta confirma el alcance acordado y que no se necesita una migración de base de datos. El siguiente paso es continuar con la especificación y el diseño antes de implementar.

## 7. Completar la planificación en modo build

Después de crear `proposal.md`, los otros artefactos todavía no se habían generado. Para completar la planificación se solicitó en modo build:

```text
Continúa y completa la planificación de add-staff-account-page.

NOTA IMPORTANTE: esto debido a que no se crearon los artefactos:
1. Especificación staff-account
2. design.md
3. tasks.md
```

OpenSpec respondió:

```text
Planificación completada
Cambio: add-staff-account-page
Ubicación: openspec/changes/add-staff-account-page/
Estado: 4/4 artefactos completos

Artefactos
- proposal.md: alcance y motivación.
- specs/staff-account/spec.md: requisitos y escenarios verificables.
- design.md: arquitectura, seguridad, UI y persistencia.
- tasks.md: 15 tareas de implementación y validación.

La validación estricta de OpenSpec pasó sin errores ni advertencias.
Todos los artefactos necesarios para implementar están listos.
```

### Estructura generada

```text
openspec/
└── changes/
    └── add-staff-account-page/
        ├── specs/
        │   └── staff-account/
        │       └── spec.md
        ├── .openspec.yaml
        ├── design.md
        ├── proposal.md
        └── tasks.md
```

El archivo `.openspec.yaml` es metadato del cambio y permite a OpenSpec identificar el esquema y las opciones específicas de ese cambio. Los demás archivos son los artefactos que se revisan antes de implementar:

- `proposal.md`: motivación, alcance y cambios incluidos.
- `specs/staff-account/spec.md`: requisitos y escenarios verificables.
- `design.md`: decisiones de arquitectura, seguridad, interfaz y persistencia.
- `tasks.md`: 15 tareas ordenadas para implementar y validar.

La planificación quedó completa, pero todavía no se ejecutó ninguna tarea de implementación. El siguiente paso es solicitar la aplicación del cambio:

```text
@openspec-apply-change add-staff-account-page
```

La documentación oficial puede mostrar `/openspec-apply-change`; en este proyecto la invocación configurada utiliza el prefijo `@`.

## 8. Implementar el cambio

Con los cuatro artefactos revisados, se ejecutó la implementación en modo build:

```text
@openspec-apply-change add-staff-account-page
```

OpenSpec respondió:

```text
Implementación completa
Cambio: add-staff-account-page
Progreso: 15/15 tareas completadas
```

### Funcionalidad implementada

- Nueva ruta `/account` para personal.
- Navegación lateral y móvil enlazada.
- Perfil tenant-scoped con edición de nombre y teléfono.
- Cambio de contraseña funcional.
- Notificaciones visuales deshabilitadas.
- “Ayuda y soporte” deshabilitado.
- Cierre de sesión funcional.
- Persistencia PostgreSQL limitada y segura.
- Diseño responsive y accesible.
- Documentación actualizada en `README.md`.

### Verificaciones ejecutadas

- `npx tsc --noEmit --incremental false` ✓
- `npx eslint src` ✓
- `npm run build` ✓
- Validación estricta de OpenSpec ✓
- Smoke tests de Playwright con artefactos en `.playwright-mcp/StaffAccount/` ✓

El comando `npm run lint` completo todavía reporta errores preexistentes en `references/screens/support.js` y `.playwright-mcp/Spec29/`. Esos errores están fuera del cambio `add-staff-account-page`.

El cambio está implementado y listo para revisión, verificación final y archivado.

## 9. Verificar el cambio

Con la implementación terminada se ejecutó la verificación indicando explícitamente el nombre del cambio:

```text
@openspec-verify-change add-staff-account-page
```

La verificación confirmó:

```text
Informe de verificación: add-staff-account-page
Se verificó el cambio sin modificar archivos ni archivarlo.
```

### Comprobaciones realizadas

- `openspec list --json`: proyecto OpenSpec detectado correctamente.
- `openspec status --change "add-staff-account-page" --json`: planificación completa.
- `openspec validate "add-staff-account-page" --strict`: correcto.
- `npx tsc --noEmit --incremental false`: correcto.
- `npx eslint src`: correcto.
- `npm run build`: correcto.
- Artefactos Playwright encontrados en `.playwright-mcp/StaffAccount/`.

### Resultado

| Dimensión | Resultado |
| --- | --- |
| Completitud | `15/15` tareas completadas; `4/4` artefactos disponibles. |
| Corrección | Los 8 requisitos tienen evidencia en la implementación. |
| Coherencia | Implementación alineada con la arquitectura, seguridad y aislamiento tenant-scoped. |

### Advertencias

- `npm run lint` completo todavía reporta errores preexistentes en `.playwright-mcp/Spec29/server-only-test-shim.cjs` y `references/screens/support.js`; `npx eslint src` pasa correctamente.
- `design.md` indicaba inicialmente componentes privados de la ruta, pero posteriormente fueron extraídos por el cambio independiente `extract-reusable-account-forms`. Conviene mantener esa relación documentada.

La evaluación no encontró problemas críticos. El cambio quedó listo para sincronización y archivado después de revisar las advertencias.

## 10. Archivar el cambio

Después de revisar el informe de verificación, se archivó el cambio:

```text
@openspec-archive-change add-staff-account-page
```

OpenSpec respondió:

```text
Archive Complete
Cambio: add-staff-account-page
Schema: spec-driven
Archivado en: openspec/changes/archive/2026-11-24-add-staff-account-page/
Specs: ✓ Sincronizadas a openspec/specs/staff-account/spec.md
Estado: Todos los artefactos y las 15 tareas completados.
```

La Spec delta de `staff-account` pasó a la Spec principal del proyecto y el cambio dejó de estar activo.

## 11. Estado del ejemplo

El ejemplo queda actualmente en estado **implementación completa**:

- Planificación: `4/4` artefactos completos.
- Implementación: `15/15` tareas completadas.
- Verificación: completada sin problemas críticos.
- Archivado: completado en `openspec/changes/archive/2026-11-24-add-staff-account-page/`.

El estado se refleja en el [diagrama del flujo OpenSpec](../01-fundamentos-openspec.md#11-diagrama-del-flujo).
