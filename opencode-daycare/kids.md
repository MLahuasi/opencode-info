# CREAR KIDS

1. **CREAR SPEC**

- Se van a crear la páginas `kids` en base a la `referencia` [`ninos.dc.html`](../open-daycare/references/screens/ninos.dc.html) y `kid-profile` en base a la `referencia` [`perfil-nino.dc.html`](../open-daycare/references/screens/perfil-nino.dc.html)
- En modod `Plan` especificar:

```text
/spec

Implementa la página kids basada en la plantilla @references/screens/ninos.dc.html . Debe ser accesible desde StaffSidebar mediante la opción children y usar la ruta `/kids`. Desde kids, al seleccionar un niño, debe abrirse la página kid-profile, basada en la plantilla @references/screens/perfil-nino.dc.html , mediante la ruta dinámica `/kids/[slug]`. Considera las especificaciones definidas en @AGENTS.md
```

- Estos son los acuerdos a los que llegamos con `OpenCode` para generar los `specs`:

```md
### Alcance Y Navegación

1. Implementar el listado de niños en /kids, basado en references/screens/ninos.dc.html.
2. Implementar el perfil en /kids/[slug], basado en references/screens/perfil-nino.dc.html.
3. Los ocho niños tendrán un perfil navegable y datos ficticios diferentes.
4. Los slugs usarán el nombre completo en ASCII, por ejemplo mateo-fernandez.
5. Un slug inexistente mostrará una página 404 propia con enlace de regreso a /kids.
6. StaffSidebar conservará su configuración actual y solo añadirá navegación real para Niños hacia /kids.
7. Niños estará activo en /kids y /kids/[slug]; Feed continuará activo únicamente en /.
8. En móvil se reutilizará la navegación inferior actual y el contenido se adaptará en columnas apiladas.

### Datos

9. No habrá base de datos, API ni persistencia en esta fase.
10. Los datos se almacenarán en mocks tipados que representen información obtenida de una futura capa de persistencia.
11. Ningún dato de niños o padres quedará hardcodeado dentro de páginas o componentes.
12. Kid tendrá, entre otros campos, birthDate, medicalNotes y referencias a padres mediante IDs.
13. medicalNotes será un campo editable conceptualmente dentro de Kid, pero esta spec no implementará su edición.
14. Las notas médicas se mostrarán solamente en el perfil, no como insignias del listado.
15. La edad se calculará desde birthDate; no existirá un campo de edad fijo.
16. La inicial y la cantidad de padres también se derivarán de los datos canónicos.
17. Los valores calculados tendrán prioridad sobre textos desactualizados de la plantilla; Mateo aparecerá actualmente con 4 años si conserva 2022-03-12.
18. Parent será una entidad independiente enlazada con Kid mediante IDs.
19. Parent tendrá id, name, email, relationship, code y status.
20. relationship admitirá madre, padre o tutor mediante valores de código en inglés.
21. status pertenecerá directamente a Parent y admitirá activa, inactiva y pendiente.
22. Los códigos alfanuméricos de padres serán valores fijos dentro del mock, no se regenerarán al ejecutar la aplicación.
23. No se implementarán página de padres ni funcionalidad para vincular padres.

### Organización De Mocks

24. Todos los mocks, incluidos los ya implementados, se centralizarán bajo app/data/mocks/.
25. Los mocks se agruparán en directorios por dominio: feed/, layout/ y kids/.
26. Cada directorio tendrá su propio index.ts.
27. Existirá un barrel raíz en app/data/mocks/index.ts.
28. La aplicación importará los mocks desde el barrel raíz.
29. Los tipos permanecerán en app/features/<domain>/types/; no se definirán dentro de los fixtures.
30. Los mocks existentes de Feed y StaffSidebar se moverán desde sus ubicaciones actuales.
31. Esta organización permitirá reutilizar los fixtures en futuros unit tests.
32. Esta fase no instalará ni configurará todavía una librería de pruebas.

### Listado De Niños

33. El listado mantendrá los ocho niños y la composición visual principal de la plantilla.
34. Los resúmenes visibles se obtendrán o calcularán desde los mocks.
35. El buscador realizará filtrado local por nombre.
36. El filtro ignorará diferencias de mayúsculas, minúsculas y tildes.
37. Cuando no haya coincidencias, se mostrará el grid vacío sin mensaje adicional.
38. Seleccionar una tarjeta navegará al perfil correspondiente.

### Perfil

39. Cada perfil obtendrá toda su información desde los mocks.
40. Cada niño tendrá fechas, notas médicas y padres ficticios coherentes con sus datos.
41. Los nombres, parentesco y estado de los padres se mostrarán en el perfil.
42. El perfil permitirá regresar a /kids.
43. No se implementarán edición, altas, resumen del día ni vinculación de padres.

### Controles Sin Funcionalidad

44. Agregar niño, Editar, Resumen del día y Vincular otro padre se conservarán como botones presentacionales.
45. Estos botones no tendrán enlaces, handlers ni operaciones CRUD.
46. Mantendrán estados visuales hover, active y focus-visible.

### Componentes Reutilizables

47. Se reutilizará el componente Button existente.
48. Se extraerán componentes globales reutilizables como Avatar, Badge, SearchField y LinkButton.
49. Feed y StaffSidebar se refactorizarán para usar los componentes globales cuando corresponda.
50. La refactorización no cambiará contenido, apariencia, responsive, landmarks ni comportamiento observable del Home.
51. No se creará un componente global Card.
52. Las tarjetas y estructuras específicas permanecerán dentro de cada feature.
53. Los componentes reutilizables aceptarán className, atributos nativos aplicables y JSDoc completo.

### División Del Trabajo

54. La refactorización transversal se separará de la implementación de Kids.
55. SPEC 02 centralizará los mocks y establecerá la UI global reutilizable.
56. SPEC 03 implementará el listado y los perfiles de niños.
57. SPEC 03 dependerá de SPEC 02.
58. Los nombres previstos son specs/02-shared-ui-and-mocks.md y specs/03-kids-and-profiles.md.

### Verificación

59. Ambas specs deberán pasar npx eslint app.
60. Ambas specs deberán pasar npx tsc --noEmit --incremental false.
61. Ambas specs deberán pasar npm run build.
62. Kids se verificará en escritorio y móvil, incluyendo navegación, filtro, perfiles y 404.
63. Las capturas de Kids se almacenarán bajo .playwright-mcp/Kids/.
```

- Se generaron 2 `spects` debido a cambios mas alla de lo solicitado inicialmente (mover mocks a otro directorio)
  > - [02-shared-ui-and-mocks.md](../open-daycare/specs/02-shared-ui-and-mocks.md)
  > - [03-kids-and-profiles.md](../open-daycare/specs/03-kids-and-profiles.md)
- El `spec` 3 denpende del 2

---

2. **IMPLEMENTAR SPEC**

- Solicitar crear `spec` en modo `Build`

```text
crea spec
```

- Se creó `2-shared-ui-and-mocks.md`
- Solicitar implementar `spec` creado:

```
/spec-impl @specs/02-shared-ui-and-mocks.md
```

- `Plan` especifica los siguientes pasos

```md
## Implementation plan

1. Crear la estructura `app/data/mocks/` y sus barrels de Feed, Layout y raíz, dejando exports válidos y compilables.
2. Mover el fixture de Feed a `app/data/mocks/feed/feed-mock.ts` y actualizar los imports públicos sin modificar sus valores.
3. Mover el fixture de StaffSidebar a `app/data/mocks/layout/staff-sidebar-mock.ts` y actualizar los imports públicos sin modificar su configuración.
4. Crear `Avatar` en `app/components/ui/avatar.tsx` y su estilo asociado, aceptando `className`, inicial, tratamiento visual y atributos accesibles necesarios.
5. Crear `Badge` en `app/components/ui/badge.tsx` y su estilo asociado, aceptando `className`, contenido y variante visual.
6. Crear `SearchField` y `LinkButton` como componentes presentacionales reutilizables con JSDoc, estados visuales y `className` combinable.
7. Refactorizar Feed y StaffSidebar para consumir los mocks desde `@/app/data/mocks` y reutilizar los nuevos componentes donde corresponda.
8. Revisar que el Home conserve el mismo contenido, layout, comportamiento responsive, landmarks y estados de interacción que SPEC 01.
```

---

3. **CRITERIOS DE ACEPTACIÓN**

En [02-shared-ui-and-mocks.md](../open-daycare/specs/02-shared-ui-and-mocks.md) se especificaron los siguientes:

```md
## Acceptance criteria

- [ ] Existe `app/data/mocks/index.ts` y exporta los fixtures públicos de Feed, Layout y Kids.
- [ ] Los mocks de Feed y StaffSidebar ya no se almacenan dentro de `features/<domain>/data`.
- [ ] Los imports de la aplicación consumen los mocks desde `@/app/data/mocks`.
- [ ] Los tipos de Feed y Layout permanecen en sus directorios de `features` y no se duplican dentro de los mocks.
- [ ] Existen componentes reutilizables `Avatar`, `Badge`, `SearchField` y `LinkButton` bajo `app/components/ui/`.
- [ ] Los componentes reutilizables aceptan y combinan `className`.
- [ ] Los componentes reutilizables tienen JSDoc completo para sus props propias y retorno.
- [ ] `/` conserva el copy, datos, estilos, responsive, landmarks y accesibilidad definidos en SPEC 01.
- [ ] La refactorización no introduce navegación ficticia, handlers de negocio ni estado de cliente innecesario.
- [ ] `npx eslint app` termina correctamente.
- [ ] `npx tsc --noEmit --incremental false` termina correctamente.
- [ ] `npm run build` termina correctamente.
```

4. **APROBACIÓN**

- En modo `Build` ejecutar el agente:

```bash
@spec-acceptance-validator 02-shared-ui-and-mocks.md
```

```md
## Acceptance criteria

- [x] Existe `app/data/mocks/index.ts` y exporta los fixtures públicos de Feed, Layout y Kids.
- [x] Los mocks de Feed y StaffSidebar ya no se almacenan dentro de `features/<domain>/data`.
- [x] Los imports de la aplicación consumen los mocks desde `@/app/data/mocks`.
- [x] Los tipos de Feed y Layout permanecen en sus directorios de `features` y no se duplican dentro de los mocks.
- [x] Existen componentes reutilizables `Avatar`, `Badge`, `SearchField` y `LinkButton` bajo `app/components/ui/`.
- [x] Los componentes reutilizables aceptan y combinan `className`.
- [x] Los componentes reutilizables tienen JSDoc completo para sus props propias y retorno.
- [x] `/` conserva el copy, datos, estilos, responsive, landmarks y accesibilidad definidos en SPEC 01.
- [x] La refactorización no introduce navegación ficticia, handlers de negocio ni estado de cliente innecesario.
- [x] `npx eslint app` termina correctamente.
- [x] `npx tsc --noEmit --incremental false` termina correctamente.
- [x] `npm run build` termina correctamente.
```

**IMPORTANTE**:

- Una vez que se aprobaron los criterios de aceptación si es necesario se debe actualizar `AGENTS.md` con la nueva información y versionar en la rama.
- Una vez sin cambios por versionar se debe realizar un `merge` a la rama `main`:

```bash
git switch main
git merge spec-02-shared-ui-and-mocks
```

---

- Solicitar implementar `spec` creado:

```
/spec-impl @specs/03-kids-and-profiles.md
```

- `Plan` especifica los siguientes pasos

```md
## Plan de implementación

1.  Crear tipos y API pública de la feature Kids.
2.  Crear utilidades de normalización, validación de slugs y cálculo de edad.
3.  Crear fixtures tipados de ocho niños y sus padres.
4.  Exponer los mocks mediante sus barrels públicos.
5.  Crear app/kids/layout.tsx.
6.  Crear componentes presentacionales del listado.
7.  Crear el Client Component mínimo para filtrado.
8.  Crear app/kids/page.tsx.
9.  Crear componentes presentacionales del perfil.
10. Crear app/kids/[slug]/page.tsx.
11. Añadir generateStaticParams() y 404 propia.
12. Actualizar StaffSidebar.
13. Añadir tokens semánticos y estilos responsive.
```

---

3. **CRITERIOS DE ACEPTACIÓN**

En [03-kids-and-profiles.md](../open-daycare/specs/03-kids-and-profiles.md) se especificaron los siguientes:

```md
## Acceptance criteria

- [ ] `/kids` carga sin errores de servidor, consola o hidratación.
- [ ] `/kids` muestra los ocho niños definidos por el mock y conserva la composición visual principal de `ninos.dc.html`.
- [ ] Cada tarjeta deriva su inicial, edad, cantidad de padres y cualquier etiqueta no médica desde los datos mock.
- [ ] El listado no muestra ni serializa `medicalNotes` ni ningún resumen médico.
- [ ] Las tarjetas navegan a un slug ASCII único bajo `/kids/[slug]`.
- [ ] Los ocho slugs conocidos muestran un perfil completo basado en los datos del mock.
- [ ] El perfil muestra nombre, edad calculada desde `birthDate`, sala, ingreso y `medicalNotes` del niño.
- [ ] El perfil muestra los padres asociados mediante `parentIds`, incluyendo nombre, parentesco traducido y estado traducido.
- [ ] El perfil no expone funcionalidades para modificar datos.
- [ ] El perfil permite regresar funcionalmente a `/kids`.
- [ ] Un slug desconocido muestra la página 404 propia con un enlace funcional a `/kids`.
- [ ] La 404 conserva el layout de Kids, StaffSidebar, navegación móvil, un único H1 y el enlace de regreso.
- [ ] `generateStaticParams()` devuelve los ocho slugs canónicos.
- [ ] El buscador filtra localmente por nombre ignorando mayúsculas, minúsculas y tildes.
- [ ] La consulta se recorta con `trim()` y una consulta vacía muestra todos los niños.
- [ ] Un filtro sin coincidencias deja el grid sin tarjetas y conserva visibles el buscador y el encabezado.
- [ ] El Client Component del filtro recibe únicamente `KidListItem[]` y no importa mocks ni datos sensibles.
- [ ] `email`, `code`, `medicalNotes` y `parentIds` no aparecen en el payload ni en el HTML del listado.
- [ ] En `/kids` y `/kids/[slug]`, StaffSidebar marca Niños con `aria-current="page"`.
- [ ] En `/`, StaffSidebar mantiene Feed como sección activa.
- [ ] Solo Niños tiene navegación real hacia `/kids`; las demás opciones no tienen destinos ficticios.
- [ ] Agregar niño, Editar, Resumen del día y Vincular otro padre no navegan ni ejecutan CRUD.
- [ ] Los controles presentacionales con interacción futura definida usan elementos semánticos adecuados y estados visuales aplicables.
- [ ] La experiencia funciona en escritorio y móvil con la navegación inferior existente.
- [ ] Todos los fixtures están dentro de `app/data/mocks/kids/` y se consumen mediante barrels públicos.
- [ ] Los modelos de Kids están en `app/features/kids/types/` y los componentes específicos en `app/features/kids/components/`.
- [ ] Los componentes reutilizables se importan desde `@/app/components/ui` y aceptan `className` cuando corresponda.
- [ ] Las APIs exportadas y componentes reutilizables tienen JSDoc completo.
- [ ] No existen colores, sombras o gradientes literales fuera de `app/globals.css`.
- [ ] No se implementan API, base de datos, persistencia ni datos hardcodeados en páginas o componentes.
- [ ] `npx eslint app` termina correctamente.
- [ ] `npx tsc --noEmit --incremental false` termina correctamente.
- [ ] `npm run build` termina correctamente.
- [ ] Las capturas de verificación se almacenan bajo `.playwright-mcp/Kids/`.
```

4. **APROBACIÓN**

- En modo `Build` ejecutar el agente:

```bash
@spec-acceptance-validator @specs/03-kids-and-profiles.md
```

```md
## Acceptance criteria

- [x] `/kids` carga sin errores de servidor, consola o hidratación.
- [x] `/kids` muestra los ocho niños definidos por el mock y conserva la composición visual principal de `ninos.dc.html`.
- [x] Cada tarjeta deriva su inicial, edad, cantidad de padres y cualquier etiqueta no médica desde los datos mock.
- [x] El listado no muestra ni serializa `medicalNotes` ni ningún resumen médico.
- [x] Las tarjetas navegan a un slug ASCII único bajo `/kids/[slug]`.
- [x] Los ocho slugs conocidos muestran un perfil completo basado en los datos del mock.
- [x] El perfil muestra nombre, edad calculada desde `birthDate`, sala, ingreso y `medicalNotes` del niño.
- [x] El perfil muestra los padres asociados mediante `parentIds`, incluyendo nombre, parentesco traducido y estado traducido.
- [x] El perfil no expone funcionalidades para modificar datos.
- [x] El perfil permite regresar funcionalmente a `/kids`.
- [x] Un slug desconocido muestra la página 404 propia con un enlace funcional a `/kids`.
- [x] La 404 conserva el layout de Kids, StaffSidebar, navegación móvil, un único H1 y el enlace de regreso.
- [x] `generateStaticParams()` devuelve los ocho slugs canónicos.
- [x] El buscador filtra localmente por nombre ignorando mayúsculas, minúsculas y tildes.
- [x] La consulta se recorta con `trim()` y una consulta vacía muestra todos los niños.
- [x] Un filtro sin coincidencias deja el grid sin tarjetas y conserva visibles el buscador y el encabezado.
- [x] El Client Component del filtro recibe únicamente `KidListItem[]` y no importa mocks ni datos sensibles.
- [x] `email`, `code`, `medicalNotes` y `parentIds` no aparecen en el payload ni en el HTML del listado.
- [x] En `/kids` y `/kids/[slug]`, StaffSidebar marca Niños con `aria-current="page"`.
- [x] En `/`, StaffSidebar mantiene Feed como sección activa.
- [x] Solo Niños tiene navegación real hacia `/kids`; las demás opciones no tienen destinos ficticios.
- [x] Agregar niño, Editar, Resumen del día y Vincular otro padre no navegan ni ejecutan CRUD.
- [x] Los controles presentacionales con interacción futura definida usan elementos semánticos adecuados y estados visuales aplicables.
- [x] La experiencia funciona en escritorio y móvil con la navegación inferior existente.
- [x] Todos los fixtures están dentro de `app/data/mocks/kids/` y se consumen mediante barrels públicos.
- [x] Los modelos de Kids están en `app/features/kids/types/` y los componentes específicos en `app/features/kids/components/`.
- [x] Los componentes reutilizables se importan desde `@/app/components/ui` y aceptan `className` cuando corresponda.
- [x] Las APIs exportadas y componentes reutilizables tienen JSDoc completo.
- [x] No existen colores, sombras o gradientes literales fuera de `app/globals.css`.
- [x] No se implementan API, base de datos, persistencia ni datos hardcodeados en páginas o componentes.
- [x] `npx eslint app` termina correctamente.
- [x] `npx tsc --noEmit --incremental false` termina correctamente.
- [x] `npm run build` termina correctamente.
- [x] Las capturas de verificación se almacenan bajo `.playwright-mcp/Kids/`.
```

**IMPORTANTE**:

- Una vez que se aprobaron los criterios de aceptación si es necesario se debe actualizar `AGENTS.md` con la nueva información y versionar en la rama.
- Una vez sin cambios por versionar se debe realizar un `merge` a la rama `main`:

```bash
git switch main
git merge spec-03-kids-and-profiles
```

- En este punto se protegió en `GitHub` el repositorio para se suban cambios mediante un `Pull Request`
- Se crearon varios `branchs` y se realizó un `merge` por cada uno a la rama `main`
- Para subir los cambios se crea una rama para unificar con todos los cambios:

```bash
git switch -c sync-main-changes
```

- Se suben los cambios al repositorio

```bash
git push -u origin sync-main-changes
```

- Esto crea un `Pull Request` en el repositorio de `GitHub`, se debe aprobar y eliminar la rama `sync-main-changes`
- En el repositorio local se debe actualizar `main`

```bash
git switch main
git pull
```

- Eliminar las ramas que ya no se usan

```bash
git branch -d spec-02-shared-ui-and-mocks
git branch -d spec-03-kids-and-profiles
git branch -d spec-04-kid-allergy-tags
git branch -d sync-main-changes
```
