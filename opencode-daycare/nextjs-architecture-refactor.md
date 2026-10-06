### Refactorizar Arquitectura

- En modo `Plan`

```
/spec

Reorganiza la arquitectura actual del proyecto para separar claramente routing y páginas, componentes reutilizables, lógica de aplicación, dominio e infraestructura.

La arquitectura debe organizarse principalmente por capas y, dentro de cada capa, por dominio, subdominio o funcionalidad cuando corresponda.

Arquitectura objetivo:

src/
├── app/                              # Páginas, layouts, rutas, Route Handlers y adapters de entrada
│   ├── (staff)/                      # Páginas exclusivas del personal
│   ├── (general)/                    # Páginas familiares y compartidas entre roles
│   ├── auth/                         # Autenticación y onboarding público
│   └── api/
│
├── application/                      # Casos de uso, consultas, comandos, DTOs y ports
│   ├── auth/
│   ├── family/
│   │   └── feed/
│   ├── kid/
│   └── post/
│
├── domain/                           # Entidades, modelos y reglas puras de negocio
│   ├── auth/
│   ├── family/
│   │   └── feed/
│   ├── kid/
│   └── post/
│
├── infrastructure/                   # Implementaciones concretas y recursos externos
│   ├── adapters/
│   │   ├── cloudinary/
│   │   ├── mailer/
│   │   └── password/
│   ├── auth/
│   ├── composition/
│   ├── config/
│   └── persistence/
│
└── components/
    ├── ui/                           # Controles UI genéricos y reutilizables
    ├── layout/                       # Layout, navegación y shells globales
    └── domain/                       # Componentes funcionales reutilizables ligados al dominio
        └── post/

No crear directorios vacíos únicamente para completar esta estructura. Cada carpeta debe existir solo cuando tenga una responsabilidad concreta.

## Routing y páginas

- Mantener en `app` únicamente routing, páginas, layouts, Route Handlers, adapters de entrada y colaboradores privados de una ruta.
- Permitir `_components` y `_actions` junto a una ruta cuando sean exclusivos de esa ruta o de un ancestro común.
- Los componentes y Server Actions colocados en `app` no deben contener lógica de negocio; deben delegar en `application`.
- Agrupar las páginas mediante Route Groups según el área de acceso.
- `(staff)` debe contener exclusivamente páginas destinadas al personal.
- `(general)` debe contener páginas familiares y páginas autenticadas compartidas entre roles.
- Las rutas de autenticación y onboarding público deben permanecer separadas de `(staff)` y `(general)`.

## Layouts por Route Group

- Crear `layout.tsx` a nivel de Route Group únicamente cuando exista comportamiento realmente compartido.
- `(staff)/layout.tsx` puede centralizar shell, navegación y controles de acceso comunes al personal.
- `(general)/layout.tsx` debe existir únicamente si sus páginas comparten composición, navegación o comportamiento común.
- Las rutas de autenticación/onboarding pueden tener layout propio si existe una responsabilidad compartida real.
- Evitar layouts vacíos creados únicamente por simetría.
- Los layouts no deben contener lógica de negocio.

## Rutas de Post

Reorganizar `post`, `post-detail` y `post-comment` bajo una única jerarquía de Posts.

Usar como rutas definitivas:

- `/posts/new`
- `/posts/[postId]`
- `/posts/[postId]/edit`
- `/posts/[postId]/comments/new`
- `/posts/[postId]/comments/[commentId]/edit`

Las rutas antiguas:

- `/post`
- `/post-detail`
- `/post-comment/new`
- `/post-comment/edit`

deben eliminarse cuando no exista una necesidad externa real de compatibilidad.

No mantener aliases, rutas duplicadas o nombres legacy únicamente por compatibilidad interna.

Actualizar enlaces, redirects internos, destinos de cancelación y llamadas a `revalidatePath` para utilizar exclusivamente las nuevas rutas.

Las nuevas rutas deben considerarse las rutas oficiales del sistema.

## Flujo de invitación Parent/Tutor ↔ Kid

Separar claramente el inicio del flujo por personal de la aceptación pública por padre o tutor.

### Inicio por personal

El personal inicia la invitación desde el perfil del niño.

Usar una ruta equivalente a:

`/kids/[slug]/invite-parent`

ubicada bajo `(staff)`.

Esta ruta:

- pertenece al área de acceso del personal;
- utiliza el niño identificado por `[slug]` como contexto;
- puede resolver internamente el ID persistido necesario;
- inicia la creación y envío de la invitación.

### Aceptación por padre o tutor

El padre o tutor recibe un enlace por email y todavía puede no tener credenciales ni una sesión autenticada.

Usar como ruta pública:

`/auth/parent-invitation/[token]`

Esta ruta debe permitir:

- validar la invitación;
- verificar el destinatario;
- crear o activar credenciales;
- completar la vinculación Parent/Tutor ↔ Kid.

Puede existir:

`/auth/parent-invitation`

como página informativa pública para explicar que el usuario debe utilizar el enlace recibido por email.

### Compatibilidad de enlaces enviados previamente

Los enlaces antiguos de activación que puedan existir en emails ya enviados, por ejemplo:

- `/auth/activate-account?code=<token>`
- `/activate-account?code=<token>`

pueden conservar redirects hacia:

`/auth/parent-invitation/[token]`

porque representan compatibilidad externa real con enlaces que ya salieron del sistema.

No mantener compatibilidad legacy cuando no exista este tipo de consumidor externo.

## Ownership de Parent/Tutor ↔ Kid

Separar ubicación de las rutas de ownership funcional.

- La relación Parent/Tutor ↔ Kid pertenece al dominio `family`.
- Las reglas puras de esa relación deben vivir en `domain/family`.
- Los casos de uso de creación, aceptación y finalización de la relación deben vivir en `application/family`.
- La invitación pertenece funcionalmente a Family aunque sea iniciada desde una página de Kids.
- Las operaciones propias del niño deben permanecer en `application/kid` o `domain/kid`.
- La creación, activación y validación de credenciales debe permanecer en `application/auth` o `domain/auth` cuando corresponda.
- No mover toda la funcionalidad a Auth solo porque el flujo cree credenciales.
- No mover toda la funcionalidad a Kid solo porque el proceso se inicie desde el perfil de un niño.
- Family, Kid y Auth deben colaborar sin duplicar responsabilidades ni crear dependencias circulares.

## Organización por capas

- Eliminar `features` como capa intermedia.
- Eliminar carpetas ambiguas como `shared`, distribuyendo su contenido según su responsabilidad real.
- Organizar cada capa primero por dominio y luego por subdominio o funcionalidad cuando corresponda.
- Preferir jerarquías como `family/feed` en lugar de nombres compuestos como `family-feed`.
- Integrar las responsabilidades de `post-detail`, comentarios, reacciones y otras operaciones de publicaciones bajo `post` cuando pertenezcan al mismo dominio.

## Application

Colocar en `application`:

- casos de uso;
- comandos;
- consultas;
- orquestación;
- validación de entrada;
- DTOs y proyecciones;
- ports requeridos por los casos de uso.

Mantener páginas, redirects, respuestas HTTP, React y APIs específicas del framework fuera de Application.

## Domain

Colocar en `domain`:

- entidades;
- modelos;
- value objects cuando sean necesarios;
- reglas de negocio;
- predicados y comportamiento puro.

Domain no debe depender de:

- Application;
- Infrastructure;
- Next.js;
- React;
- NextAuth;
- filesystem;
- SDKs externos;
- implementaciones concretas.

## Components

Organizar los componentes en:

`components/ui`
- controles visuales genéricos;
- reutilizables;
- sin conocimiento del dominio.

`components/layout`
- header;
- sidebar;
- navegación;
- shells y composición global.

`components/domain`
- componentes React reutilizables entre distintas rutas o áreas;
- ligados a conceptos funcionales del dominio.

Por ejemplo:

`components/domain/post/FeedPostCard`

Los componentes exclusivos de una sola ruta deben permanecer en el `_components` más cercano dentro de `app`.

No colocar componentes React dentro de `application` o `domain`.

## Infrastructure

Colocar en `infrastructure`:

- persistencia;
- filesystem;
- APIs externas;
- providers;
- SDKs;
- configuración server-only;
- NextAuth concreto;
- implementaciones de ports;
- composición de dependencias.

Organizar adapters externos, por ejemplo:

- `infrastructure/adapters/cloudinary`
- `infrastructure/adapters/mailer`
- `infrastructure/adapters/password`

La persistencia concreta debe permanecer bajo `infrastructure/persistence`.

## Inversión de dependencias

Aplicar inversión de dependencias únicamente cuando exista un servicio, proveedor o recurso externo intercambiable.

Criterios:

- `application` o `domain` deben depender de interfaces/ports y no de implementaciones concretas.
- El port debe definirse en la capa y módulo que necesita la dependencia.
- Los adapters concretos deben implementarse en `infrastructure`.
- Evitar que Application o Domain conozcan filesystem, bases de datos, JSON, Cloudinary, Mailer, bcrypt, NextAuth concreto u otros providers.
- No crear un port por cada archivo físico o colección de persistencia.
- Diseñar los contratos según las necesidades de los casos de uso.

Ejemplos válidos de ports:

- repositorios de persistencia;
- `ImageStorage`;
- `InvitationMailer`;
- `PasswordHasher`.

No crear ports o wrappers innecesarios para APIs nativas del framework o del runtime cuando no exista una necesidad real de reemplazo, por ejemplo:

- `redirect`;
- `revalidatePath`;
- `notFound`;
- cookies;
- `Date`;
- `crypto.randomUUID`.

Mantener esas APIs en la capa de entrada o donde corresponda sin abstraerlas artificialmente.

## Composición

Crear un punto de composición server-only en:

`src/infrastructure/composition/`

Organizar la composición por dominio o flujo, por ejemplo:

- `auth`
- `family`
- `kid`
- `post`

La composición debe:

- instanciar adapters concretos;
- conectarlos con los ports requeridos por Application;
- exponer casos de uso u operaciones ya ensambladas;
- evitar que páginas y Server Actions construyan repositories o SDKs directamente.

No utilizar un contenedor DI o Service Locator salvo que exista una necesidad posterior explícita.

## Principios generales

- Mantener páginas, Route Handlers y Server Actions pequeños.
- Delegar la lógica funcional a `application`.
- Evitar dependencias circulares entre dominios.
- Evitar duplicación.
- Evitar capas, interfaces, wrappers y factories sin responsabilidad real.
- Mantener nombres de carpetas consistentes y en minúsculas.
- No modificar reglas de negocio durante la reorganización.
- No modificar datos persistidos salvo que otra spec lo solicite explícitamente.
- No añadir dependencias únicamente para validar la arquitectura.
- Utilizar ESLint, TypeScript, build, búsquedas estáticas y las pruebas existentes para validar los cambios.

## Generación de specs

Dividir la reorganización en specs incrementales y ejecutables cuando sea necesario para reducir el riesgo.

Los specs deben llevar progresivamente el proyecto hacia la arquitectura objetivo; no deben crear estructuras temporales como `features` o `shared` que ya se sabe que serán eliminadas.

Cada spec debe indicar claramente:

- objetivo;
- alcance;
- fuera de alcance;
- estructura afectada;
- cambios requeridos;
- orden de implementación;
- criterios de aceptación;
- decisiones arquitectónicas;
- riesgos;
- dependencias respecto de otras specs.

No implementar código durante la generación de los specs.

No replantear decisiones arquitectónicas ya definidas salvo que el análisis del proyecto revele un conflicto técnico real; en ese caso, documentar el conflicto antes de proponer una alternativa.
```

- Una vez concluido en modo `Build`

```text
crea spec
```

- Analizar el spec creado [16-layered-architecture-foundation.md](../../open-daycare/specs/16-layered-architecture-foundation.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/16-layered-architecture-foundation.md
```

- Cuando crea la rama `spec-16-layered-architecture-foundation` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-16-layered-architecture-foundation"
```

- También se creó el plan:

```md
## Plan de implementación

1. Registrar hashes byte a byte de los JSON y una línea base de imports.
2. Crear src/components/ui/ y trasladar sus componentes.
3. Crear src/components/layout/ y trasladar la composición estructural.
4. Crear src/infrastructure/persistence/json/ y mover el adapter.
5. Trasladar los JSON y actualizar la resolución física.
6. Distribuir utilidades y configuración transversal.
7. Actualizar consumidores y eliminar archivos antiguos sin referencias.
8. Ejecutar checks y validar Home, Auth, Kids, invitaciones y feeds.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-16-layered-architecture-foundation - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/16-layered-architecture-foundation.md
```

- Crear `pull request`

```bash
git push -u origin spec-16-layered-architecture-foundation
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-16-layered-architecture-foundation
```

---

- Analizar el spec creado [17-kid-person-room-layers.md](../../open-daycare/specs/17-kid-person-room-layers.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/17-kid-person-room-layers.md
```

- Cuando crea la rama `spec-17-kid-person-room-layers` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-17-kid-person-room-layers"
```

- También se creó el plan:

```md
## Plan de implementación

1. Registrar imports, rutas y comportamiento actual de Kids.
2. Crear modelos puros de Kid, Person y Room.
3. Crear ports de lectura y escritura necesarios.
4. Implementar repositorios JSON y composición server-only.
5. Migrar consultas y comandos a src/application/kid/.
6. Mover componentes exclusivos de Kids a \_components.
7. Reducir páginas y Server Actions a adapters delgados.
8. Eliminar features antiguas y redirect legacy.
9. Ejecutar ESLint, TypeScript, build, búsquedas estáticas y Playwright.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-17-kid-person-room-layers - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/17-kid-person-room-layers.md
```

- Crear `pull request`

```bash
git push -u origin spec-17-kid-person-room-layers
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-17-kid-person-room-layers
```

---

- Analizar el spec creado [18-family-invitations-and-auth-layers.md](../../open-daycare/specs/18-family-invitations-and-auth-layers.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/18-family-invitations-and-auth-layers.md
```

- Cuando crea la rama `spec-18-family-invitations-and-auth-layers` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-18-family-invitations-and-auth-layers"
```

- También se creó el plan:

```md
## Plan de implementación

1.  Registrar URLs, estados y transacciones existentes.
2.  Crear entidades y reglas puras de Auth y Family.
3.  Crear casos de uso de invitación, token, aceptación y activación.
4.  Definir ports de mailer, hash y persistencia.
5.  Implementar adapters y composición server-only, incluido NextAuth.
6.  Crear página protegida de invitación y sus actions.
7.  Crear páginas pública informativa y de aceptación por token.
8.  Actualizar mailer y destinos de activación.
9.  Configurar redirects legacy condicionales.
10. Eliminar rutas /auth/link-parent y /login.
11. Ejecutar checks y pruebas Playwright completas.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-18-family-invitations-and-auth-layers - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/18-family-invitations-and-auth-layers.md
```

- Crear `pull request`

```bash
git push -u origin spec-18-family-invitations-and-auth-layers
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-18-family-invitations-and-auth-layers
```

---

- Analizar el spec creado [19-post-domain-and-canonical-routes.md](../../open-daycare/specs/19-post-domain-and-canonical-routes.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/19-post-domain-and-canonical-routes.md
```

- Cuando crea la rama `spec-19-post-domain-and-canonical-routes` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-19-post-domain-and-canonical-routes"
```

- También se creó el plan:

```md
## Plan de implementación

1.  Registrar rutas, enlaces, redirects, cancelaciones y revalidaciones actuales.
2.  Crear modelos y reglas puras de Post, media, comentarios y reacciones.
3.  Crear casos de uso y proyecciones.
4.  Definir ports y composición de Post.
5.  Mover Cloudinary al adapter de Infrastructure.
6.  Mover componentes reutilizables ligados a Post.
7.  Crear las rutas staff de creación y edición.
8.  Crear detalle y jerarquía de comentarios bajo general.
9.  Actualizar todos los destinos a /posts/....
10. Eliminar features y rutas legacy tras búsquedas y smoke tests.
11. Ejecutar checks completos y pruebas Playwright.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-19-post-domain-and-canonical-routes - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/19-post-domain-and-canonical-routes.md
```

- Crear `pull request`

```bash
git push -u origin spec-19-post-domain-and-canonical-routes
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-19-post-domain-and-canonical-routes
```

---

- Analizar el spec creado [20-family-feed-and-access-groups.md](../../open-daycare/specs/20-family-feed-and-access-groups.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/20-family-feed-and-access-groups.md
```

- Cuando crea la rama `spec-20-family-feed-and-access-groups` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-20-family-feed-and-access-groups"
```

- También se creó el plan:

```md
## Plan de implementación

1. Registrar reglas actuales de autorización, filtros, anuncios, orden y rutas auxiliares.
2. Extraer predicados puros a src/domain/family/feed.
3. Crear consultas y proyecciones en src/application/family/feed.
4. Conectar persistencia, sesión y ports desde src/infrastructure/composition/family.
5. Mover Family Feed bajo (general) conservando URLs.
6. Mover sidebar, navegación y composición estructural a src/components/layout.
7. Colocar componentes exclusivos junto a sus rutas y simplificar páginas.
8. Eliminar features/family y shared tras comprobar cada responsabilidad.
9. Ejecutar checks completos y verificar Family Feed, Posts compartidos y redirects auxiliares.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-20-family-feed-and-access-groups - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/20-family-feed-and-access-groups.md
```

- Crear `pull request`

```bash
git push -u origin spec-20-family-feed-and-access-groups
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-20-family-feed-and-access-groups
```

---

- Analizar el spec creado [21-presentation-composition-and-legacy-cleanup.md](../../open-daycare/specs/21-presentation-composition-and-legacy-cleanup.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/21-presentation-composition-and-legacy-cleanup.md
```

- Cuando crea la rama `spec-21-presentation-composition-and-legacy-cleanup` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-21-presentation-composition-and-legacy-cleanup"
```

- También se creó el plan:

```md
## Plan de implementación

1. Registrar dependencias actuales entre capas y consumidores runtime.
2. Crear src/presentation/ y trasladar UI, layout y componentes Post.
3. Trasladar navegación y configuración visual a Presentation.
4. Crear src/composition/ y trasladar composiciones.
5. Eliminar dependencias legacy desde Infrastructure y Composition.
6. Conectar rutas y Server Actions con Application mediante Composition.
7. Migrar schemas, presenters y view models fuera de Domain/Application.
8. Eliminar capas legacy tras verificar referencias.
9. Ejecutar auditoría completa.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-21-presentation-composition-and-legacy-cleanup - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/21-presentation-composition-and-legacy-cleanup.md
```

- Crear `pull request`

```bash
git push -u origin spec-21-presentation-composition-and-legacy-cleanup
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-21-presentation-composition-and-legacy-cleanup
```

---

- Analizar el spec creado [22-src-app-architecture-cutover.md](../../open-daycare/specs/22-src-app-architecture-cutover.md), si hay ajustes solicitarlos, en caso contrario cambiar a estado `Approved`

- Modo `Build`

```text
/spec-impl @specs/22-src-app-architecture-cutover.md
```

- Cuando crea la rama `spec-22-src-app-architecture-cutover` relizar el primer commit:

```bash
git add .
git commit -m "DayCare - spec-22-src-app-architecture-cutover"
```

- También se creó el plan:

```md
## Plan de implementación

1. Registrar árbol, alias, rutas, redirects y hashes JSON.
2. Confirmar SPEC 21 y ausencia de estructuras obsoletas.
3. Confirmar dependencias y límites arquitectónicos.
4. Mover app/ a src/app/.
5. Actualizar tsconfig.json, NextAuth, auth.ts e imports.
6. Verificar detección de rutas, layouts y handlers por Next.js.
7. Actualizar README.md y AGENTS.md.
8. Ejecutar auditoría estática y corregir incumplimientos.
9. Ejecutar ESLint, TypeScript, build, búsquedas, comparación JSON y Playwright.
```

- en modo `Build` se debe realizar cada paso, verificar los cambios y ejecutar un commit si todo esta correcto o solictar ajustes.

```bash
git add .
git commit -m "DayCare - spec-22-src-app-architecture-cutover - paso"
```

- En OpenCode ejecutar agente `@spec-acceptance-validator` para validar `spec`

```bash
@spec-acceptance-validator @specs/22-src-app-architecture-cutover.md
```

- Crear `pull request`

```bash
git push -u origin spec-22-src-app-architecture-cutover
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada ``
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-22-src-app-architecture-cutover
```

---
