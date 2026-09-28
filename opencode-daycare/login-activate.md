## ACTIVAR CUENTA Y LOGIN

1. En modo `Plan` solicitar

```text
/spec

En base a la plantilla `@references/screens/activar-cuenta.dc.html`, se debe crear la pantalla `activate-account`, que se mostrará cuando al usuario le llegue un mensaje a su email. La información se cargará de forma automática.

Para esto se deben realizar los siguientes cambios:

* El nombre del type `Parent` debe cambiarse a `Person`. Además, se debe agregar un parámetro `role`, cuyos valores pueden ser `parent | personal`. El campo `relationship` debe admitir `null` en caso de que `role` sea `personal`. Se deben quitar los campos `relationship` y `code`.
* Se debe crear un type `ParentKid` con los campos `id`, `parentId`, `kidId`, `relationship` (`ParentRelationship`) y `photoSharingConsent` (`boolean`). En este campo se registra: “Autorizo a la guardería a tomar y compartir fotos de mi hijo dentro de la app”.

  * **NOTA:** Si el niño tiene más de un padre y uno aprueba y el otro no, se entiende que no existe autorización.
* En `Kid` se debe agregar el campo `status: "active" | "inactive"`.
* Se debe crear el type `Invitation` con los campos `id`, `personId`, `code`, `expiresAt` (`Date`) y `acceptedAt` (`Date | null`).
* Se debe crear el type `Credential` con los campos `id`, `personId` y `passwordHash`. Este tipo almacena las credenciales del usuario.
* Al dar clic en **"Iniciar sesión"**, se debe dirigir a la pantalla `login`.
* Al dar clic en **"Activar mi cuenta"**, se debe dirigir a la ruta `familia-feed`. Esta ruta no se implementará todavía.
* La card con los datos del niño puede cargarse o no con información, ya que `activate-account` también puede abrirse desde `login`, mediante la opción **"Activa tu cuenta"**.
* La ruta para cargar la página debe ser `activate-account`.

En base a la plantilla `@references/screens/login.dc.html`, se debe crear la pantalla `login`:

* La ruta de la página debe ser `login`.
* Al dar clic en **"Iniciar sesión"**, se debe abrir `home`.
* Al dar clic en **"Activa tu cuenta"**, se debe abrir `activate-account`.
* No se debe considerar en el diseño la sección **"INGRESO COMO"** (`Personal`, `Familia`).

**IMPORTANTE:**

* No se debe usar español de Argentina. Se debe usar español de Ecuador (`EC`), y este parámetro debe ser configurable globalmente.
* Se deben seguir las especificaciones definidas en `@AGENTS.md`.
* Ajusta y crea nuevos mocks con estos cambios
* Aún no se implementará conexión BDD
* Tampoco se crearán unit test

```

- En modo `Build`:

```text
crea spec
```

- Una vez generado el `spec` verificar y cambiar su estado a `Approved`
- En modo `Build` empezar la implementación

```text
/spec-impl @specs/05-account-activation-and-login.md
```

- Se define los siguientes pasos:

```md
1.  Crear y aplicar APP_LOCALE.
2.  Actualizar los tipos de Kids.
3.  Migrar mocks de padres a personas.
4.  Crear mocks ParentKid.
5.  Añadir estado a los niños.
6.  Actualizar listado, perfiles y relaciones.
7.  Crear helper de consentimiento fotográfico.
8.  Crear tipos y utilidades de Auth.
9.  Crear mocks de invitaciones y credenciales.
10. Crear componentes de Auth.
11. Crear /login.
12. Crear /activate-account.
13. Implementar validación y navegación.
14. Aplicar estilos y tokens semánticos.
15. Corregir copy argentino.
16. Actualizar documentación y verificar.
```

- Se creo el branch `spec-05-account-activation-and-login`, hacer commit inicial:

```bash
git add .
git commit -m "DayCare - spec-05-account-activation-and-login"
```

- Se hace commit de cada uno de los pasos
- Una vez finalizados los pasos se cambia el `spec` a estado `Implemet`

- Para la aprobación en modo `Build` ejecutar el agente:

```bash
@spec-acceptance-validator @specs/05-account-activation-and-login
```

- Subir cambios a `GitHub` (`Pull Request`):

```bash
git push -u origin spec-05-account-activation-and-login
```

- Aprobar `Pull Request` en `GitHub` y eliminar rama creada `spec-05-account-activation-and-login`
- Actualizar git local:

```bash
git switch main
git pull
```

- Eliminar ramas

```bash
git branch -d spec-05-account-activation-and-login
```
