### Crear y Actualizar Post

- En modo `Plan`

```
/spec

Crear la página `post` para crear nuevas publicaciones al dar clic en la sección **"Comparte un momento"** de `/home`.
La página `post`, tanto para crear como para editar, debe basarse en la plantilla `@references/screens/crear-publicacion.dc.html`.
Las publicaciones deben persistirse en `feed.json`.
Crear componentes reutilizables para agregar fotos. Se debe implementar la funcionalidad de búsqueda/selección de imágenes. Las imágenes se subirán posteriormente a un repositorio en la nube Cloudinary; por ahora se debe almacenar el `path` de la imagen y persistir esta información en un JSON.
Las publicaciones creadas deben listarse en `/home`.
Cuando un padre de familia ingrese al sistema, debe acceder a `/family-feed`, basado en la plantilla `@references/screens/familia-feed.dc.html`.
Por defincion de la plantilla `@references/screens/familia-feed.dc.html` un padre de familia puede filtrar las pulicaciones por cada niño de la room o por la room, no solo de su niño.
Configurar el navbar para que cambie cuando ingrese un padre de familia. Crear únicamente las opciones necesarias que todavía no existan.
Al dar clic en una publicación, se debe navegar a `/post-detail`, basado en la plantilla `@references/screens/detalle-publicacion.dc.html`.
La información necesaria para el detalle de la publicación debe persistirse en un JSON.
```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [12-post-creation-and-editing.md](../../open-daycare/specs/12-post-creation-and-editing.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/12-post-creation-and-editing.md
```

- Cuando crea la rama `spec-12-post-creation-and-editing` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-12-post-creation-and-editing"
```

- También se creó el plan:

```md
## Plan de implementación

1.  Migrar contrato de FeedPost, posts existentes y adapter JSON.
2.  Crear staff-rooms.json, tipos y servicios de salas autorizadas.
3.  Crear adapter server-only de Cloudinary.
4.  Crear schemas de destino, tipo, descripción, imágenes y modo.
5.  Crear selector reutilizable de imágenes.
6.  Crear formulario responsive.
7.  Crear acciones server-only de creación y edición.
8.  Implementar rollback y eliminación de assets retirados.
9.  Crear rutas /post y conectarlas con home.
10. Verificar criterios funcionales, responsive y de accesibilidad.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-12-post-creation-and-editing - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-12-post-creation-and-editing
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-12-post-creation-and-editing
```

---

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

- Analizar el spec creado [13-family-room-feed.md](../../open-daycare/specs/13-family-room-feed.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/13-family-room-feed.md
```

- Cuando crea la rama `spec-13-family-room-feed` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-13-family-room-feed"
```

- También se creó el plan:

```md
## Plan de implementación

1. Crear servicios server-only para el contexto familiar, salas, niños y filtros.
2. Ampliar la autorización para incluir niños activos de las salas autorizadas.
3. Crear configuración y componentes de navegación familiar.
4. Crear el servicio de filtrado, deduplicación y orden cronológico.
5. Crear /family-feed y sus rutas placeholder.
6. Actualizar login y accesos directos según el rol.
7. Conectar tarjetas al detalle y proyectar URLs firmadas autorizadas.
8. Verificar con Playwright y ejecutar las comprobaciones del proyecto.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-13-family-room-feed - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-13-family-room-feed
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-13-family-room-feed
```

---

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

- Analizar el spec creado [14-post-detail.md](../../open-daycare/specs/14-post-detail.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/14-post-detail.md
```

- Cuando crea la rama `spec-14-post-detail` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-14-post-detail"
```

- También se creó el plan:

```md
## Implementation Plan

1. Crear tipos, colecciones JSON y fixtures de comentarios y reacciones.
2. Crear servicios server-only para consultar un post y sus relaciones con autorización.
3. Crear la proyección de detalle por rol, incluyendo media Cloudinary autorizada.
4. Crear el componente visual de detalle.
5. Crear /post-detail?id=<id> con retorno según el rol.
6. Actualizar las tarjetas de ambos feeds para navegar mediante el ID.
7. Añadir estados de no encontrado, no autorizado, sin comentarios y sin imágenes.
8. Verificar autorización, datos, Cloudinary, navegación, responsive y accesibilidad.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-14-post-detail - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-14-post-detail
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-14-post-detail
```

---

## AJUSTE

- En modo `Plan`

```
/spec

Desde /family-feed, en el menú Feed, actualmente no es posible dar like al hacer clic en el ícono de corazón ni agregar un comentario al dar clic en el ícono comentario.

* El ícono de corazón y el ícono de comentario deben implementarse como componentes reutilizables dentro de @components/ui.
* Al hacer clic en el ícono de comentario, se debe navegar a /post-comment/new y, al guardar, agregar un registro en feed-comments.json.
* Al hacer clic en el ícono de corazón, se debe actualizar feed-reactions.json con la reacción correspondiente al post.
* La reacción y el comentario deben quedar asociados al post correspondiente.
* Mantener la estructura y convenciones existentes del proyecto.
* Se debe implementa la opción /post-comment/edit y delete
* También el padre de familia puede quitar su like
```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [15-family-feed-engagement.md](../../open-daycare/specs/15-family-feed-engagement.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/15-family-feed-engagement.md
```

- Cuando crea la rama `spec-15-family-feed-engagement` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-15-family-feed-engagement"
```

- También se creó el plan:

```md
## Plan de implementación

1.  Crear y exportar HeartIcon; reutilizar iconos UI en FeedPostCard.
2.  Agregar updatedAt a comentarios y validar/migrar datos.
3.  Crear servicios de engagement y retirar contadores duplicados.
4.  Actualizar proyecciones del feed familiar, personal y detalle.
5.  Implementar toggle atómico de reacciones.
6.  Conectar el corazón en /family-feed con estados pending y error.
7.  Implementar creación de comentarios y /post-comment/new.
8.  Implementar edición y /post-comment/edit.
9.  Implementar eliminación física con confirmación.
10. Actualizar /post-detail con acciones y orden de comentarios.
11. Revalidar rutas y verificar accesibilidad, autorización y responsive.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-15-family-feed-engagement - paso"
```

- Se debe realizar una validación manual, si no hay inconvenientes cambiar a estadp `Implement`
- Crear `pull request`

```bash
git push -u origin spec-15-family-feed-engagement
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-15-family-feed-engagement
```
