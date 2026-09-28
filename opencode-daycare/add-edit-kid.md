## Añadir y Editar Nino

- En modo `Plan`

```
/spec

Crear una página para agregar y actualizar un niño basada en la plantilla `@references/screens/agregar-nino.dc.html`.
La referencia `@references/screens/agregar-nino.dc.html` debe utilizarse tanto para crear un niño desde `@references/screens/ninos.dc.html` (mediante `+ Agregar niño`) como para editarlo desde `@references/screens/perfil-nino.dc.html` (mediante `Editar`).
Los campos `Nombre completo`, `Fecha de Nacimiento` y `Sala` de `@references/screens/agregar-nino.dc.html` deben ser obligatorios, deben incluir validación y una señal visual que permita identificar que son requeridos.
El campo `Fecha de Nacimiento` debe tener una máscara de entrada para evitar el ingreso incorrecto de la información.
Se debe crear un nuevo type llamado Room con los campos id y name.
Se debe crear un mock con tres salas: Soles, Girasoles y Patitos. Este mock se utilizará para cargar las salas disponibles en la nueva página.
Se debe cambiar el campo room de Kid por roomId.
Por el momento, roomId representará la sala actual del niño. Cuando se implemente la base de datos y sea necesario mantener el historial de salas por las que ha pasado un niño, se deberá crear una relación entre Kid y Room que incluya los campos startDate y endDate para representar el período de permanencia en cada sala.
Se debe modificar el mock de kids para asignar como roomId el identificador correspondiente a la sala Soles
Los campos `ALERGIAS (ETIQUETAS)` y `NOTAS MÉDICAS` son opcionales.
Para el campo `ALERGIAS (ETIQUETAS)`, implementar un `TagsInput`. Actualmente, el campo `allergies` de `kid` es un `string` y las etiquetas se separan por `,`.
Cuando los campos estén vacíos, deben mostrar un `placeholder`.
En modo Edit, la pantalla debe recibir un parámetro `id` para buscar al niño y cargar su información.
Se deben reutilizar los componentes de `components/ui` cuando sea factible.
En caso de ser necesario, se deben crear nuevos componentes reutilizables.
Actualmente no se implementará conexión a BDD.
Tampoco se implementarán pruebas unitarias.
Para poder validar el Add y Update, se deben modificar parcialmente los mocks. Los datos deben almacenarse en archivos `.json`.
Se deben agregar alergias al menos a 4 niños, y al menos 2 de ellos deben tener 2 alergias.

```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [06-kid-create-and-update.md](../../open-daycare/specs/06-kid-create-and-update.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/06-kid-create-and-update.md
```

- Cuando crea la rama `spec-06-kid-create-and-update` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-06-kid-create-and-update"
```

- También se creó el plan:

```md
### Implementation Plan

1.  Actualizar Kid, crear Room y actualizar barrels.
2.  Crear rooms.json.
3.  Migrar los fixtures actuales a JSON.
4.  Incorporar roomId y alergias acordadas.
5.  Crear el servicio server-only de lectura y relaciones.
6.  Crear escritura atómica y operaciones Add/Update.
7.  Crear validadores compartidos.
8.  Crear y exportar TagsInput.
9.  Crear el formulario compartido de Kids.
10. Añadir máscara, accesibilidad y estados visuales.
11. Crear /kids/new.
12. Crear /kids/[id]/edit.
13. Crear las Server Actions.
14. Conectar los accesos de Add y Edit.
15. Adaptar listado, perfil y activación a roomId.
16. Corregir el agrupamiento por sala.
17. Crear y conservar el fixture final de Martina.
18. Verificar funcionalidad, responsive y accesibilidad con Playwright.
19. Ejecutar ESLint, TypeScript, build y git diff --check.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-06-kid-create-and-update - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-06-kid-create-and-update
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada `spec-05-account-activation-and-login`
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-06-kid-create-and-update
```
