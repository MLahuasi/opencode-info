# Ejemplo: refactor de formularios reutilizables

Este documento registra un refactor realizado después de completar `add-staff-account-page`. El objetivo fue eliminar la duplicación entre los formularios de cuenta familiar y de personal sin mezclar sus Server Actions ni sus reglas de autorización.

## 1. Motivo del refactor

### Prompt

```text
Se está creando un nuevo formulario para actualizar información del personal y cambio de contraseña,
y esa funcionalidad ya está implementada para familiares. Ambos formularios deben ser reutilizables
para evitar duplicidad de código. ¿Cómo se ejecuta este cambio?
```

OpenSpec identificó duplicación en:

```text
family-feed/account/_components/account-profile-form.tsx
(staff)/account/_components/account-profile-form.tsx
```

También existía duplicación en los formularios y modales de cambio de contraseña.

## 2. Decisión: cambio independiente

El cambio original `add-staff-account-page` ya estaba implementado. Por eso no se modificó ese cambio y se creó uno nuevo:

```text
extract-reusable-account-forms
```

Separar el refactor permite:

- conservar el historial de la funcionalidad original;
- revisar el cambio estructural de forma independiente;
- evitar mezclar una nueva funcionalidad con una reorganización interna;
- validar que familia y personal mantengan sus permisos.

## 3. Diseño previsto

Se propuso crear componentes reutilizables en:

```text
src/presentation/account/
├── account-profile-form.tsx
├── password-form.tsx
└── account-settings-modal.tsx
```

Los componentes recibirían mediante props:

- la Server Action correspondiente;
- el estado inicial;
- los valores actuales;
- las clases o configuración visual específica.

Las acciones continuarían separadas por contexto:

```text
Familia  → updateAccountAction / changePasswordAction
Personal → updateStaffAccountAction / changeStaffPasswordAction
```

Así se comparte la interfaz, la validación visual y los estilos comunes sin mezclar autorización ni reglas de negocio.

## 4. Crear y planificar el cambio

Se creó el cambio desde OpenCode:

```text
@openspec-new-change
```

Resultado inicial:

```text
Nuevo cambio creado
- Nombre: extract-reusable-account-forms
- Ubicación: openspec/changes/extract-reusable-account-forms/
- Workflow: spec-driven
- Estado: 0/4 artefactos completados
```

Después se solicitó crear la propuesta:

```text
Continua con la propuesta
```

La propuesta definió el refactor como un cambio sin comportamiento nuevo, manteniendo las Server Actions y los permisos específicos de familia y personal.

## 5. Usar `skip_specs: true`

Como el cambio no añade ni modifica comportamiento funcional, se configuró:

```text
Configura skip_specs: true
```

OpenSpec escribió la configuración en:

```text
openspec/changes/extract-reusable-account-forms/.openspec.yaml
```

Con `skip_specs: true`:

- no se genera `specs/**/*.md` para este cambio;
- la etapa de Specs se marca como omitida;
- el cambio conserva `proposal.md`, `design.md` y `tasks.md`;
- la validación no exige una Spec delta para un refactor sin cambio de comportamiento.

Esta opción debe utilizarse únicamente cuando el cambio no modifica el comportamiento observable. Si el refactor cambia permisos, contratos, respuestas o comportamiento del sistema, debe incluir Specs delta en lugar de omitirlas.

## 6. Completar la planificación

Se solicitó continuar:

```text
Continua con la planificación
```

Resultado:

```text
Planificación completada
Cambio: extract-reusable-account-forms
Ubicación: openspec/changes/extract-reusable-account-forms/

Artefactos:
- proposal.md
- design.md
- tasks.md
- specs: omitido mediante skip_specs: true

Estado:
- 3/3 artefactos completados.
- Validación estricta OpenSpec aprobada.
```

El plan contempló compartir el formulario de perfil, el formulario de contraseña, el modal de cuenta, los tipos compartidos y los estilos comunes.

## 7. Implementar el refactor

Se ejecutó:

```text
@openspec-apply-change extract-reusable-account-forms
```

Resultado:

```text
Refactorización completada
Cambio: extract-reusable-account-forms
Progreso: 13/13 tareas completadas
```

Se creó `src/presentation/account/` con:

- `AccountProfileForm`.
- `AccountPasswordForm`.
- `AccountModal`.
- Tipos compartidos.
- Estilos comunes.

Las cuentas familiar y de personal reutilizan ahora los mismos componentes, manteniendo sus propias Server Actions y autorizaciones.

## 8. Verificaciones

- `npx eslint src` ✓
- `npx tsc --noEmit --incremental false` ✓
- `npm run build` ✓
- Validación estricta OpenSpec ✓
- Ruta `/account` verificada.
- `/family-feed/account` redirigió correctamente porque la sesión de prueba era de personal.
- Servidor del puerto 3000 detenido.

## 9. Verificar el refactor

Se ejecutó la verificación indicando el identificador completo del cambio:

```text
@openspec-verify-change extract-reusable-account-forms
```

OpenSpec confirmó:

```text
Informe de verificación: extract-reusable-account-forms
No se modificaron archivos ni se archivó el cambio.
```

### Resultado

| Dimensión | Resultado |
| --- | --- |
| Completitud | `13/13` tareas completadas; `proposal.md`, `design.md` y `tasks.md` completos. |
| Corrección | No aplicable: el cambio declara `skip_specs: true` y no introduce comportamiento funcional. |
| Coherencia | El diseño de reutilización y separación de autorizaciones se cumple. |

### Comprobaciones realizadas

- `openspec validate extract-reusable-account-forms --strict` ✓
- `npx tsc --noEmit --incremental false` ✓
- `npx eslint src` ✓
- `npm run build` ✓
- `skip_specs: true` reconocido correctamente ✓
- No existen imports de Infrastructure, rutas o Server Actions dentro de `src/presentation/account/` ✓

### Evidencias principales

- API pública compartida: `src/presentation/account/index.ts`.
- Formularios reutilizables: `account-profile-form.tsx` y `account-password-form.tsx`.
- Modal reutilizable: `account-modal.tsx`.
- Familia conserva `updateAccountAction` y `changePasswordAction`.
- Personal conserva `updateStaffAccountAction` y `changeStaffPasswordAction`.
- Las autorizaciones de familia y personal permanecen separadas.

## 10. Archivar el refactor

Después de la verificación, se archivó el cambio de forma independiente:

```text
@openspec-archive-change extract-reusable-account-forms
```

OpenSpec respondió:

```text
Archive Complete
Cambio: extract-reusable-account-forms
Schema: spec-driven
Archivado en: openspec/changes/archive/2026-11-24-extract-reusable-account-forms/
Specs: Sin delta specs (skip_specs: true)
Estado: Todos los artefactos y las 13 tareas completados.
```

Como el cambio declara `skip_specs: true`, no se sincronizó ninguna Spec delta. El historial completo del refactor quedó conservado en `openspec/changes/archive/`.
