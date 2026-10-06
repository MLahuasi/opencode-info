# Git y protección de la rama principal

Este es un paso opcional de configuración inicial del repositorio. Protege `main` para que las funcionalidades se integren mediante Pull Requests. La sección de integración describe el Pull Request que se crea por cada `spec`.

## 1. Publicar el repositorio

Crear primero el repositorio en GitHub y configurar el remoto.

```bash
# Vincula el repositorio local con el repositorio remoto.
git remote add origin https://github.com/MLahuasi/opencode-pacman.git

# Renombra la rama principal local como main.
git branch -M main

# Publica main y configura origin/main como rama upstream.
git push -u origin main
```

---

## 2. Proteger `main`

En GitHub ingresar a:

```text
Settings / Branches
```

![Configuración de protección de la rama main](../assets/08-branch-main-block.png)

Seleccionar:

```text
Add classic branch protection rule
```

Configurar:

```text
Branch name pattern: main
```

Dentro de `Protect matching branches`:

```text
Require a pull request before merging
```

Activar:

```text
Require approvals
```

cuando se trabaja con equipos o se requiere una aprobación adicional.

También activar:

```text
Require status checks to pass before merging
```

y:

```text
Do not allow bypassing the above settings
```

Finalmente seleccionar:

```text
Create
```

A partir de este momento los cambios no deberían integrarse directamente en `main`.

Las nuevas funcionalidades deberán trabajar desde otra rama y posteriormente utilizar un Pull Request.

---

## 3. Crear una rama de trabajo

Ejemplo:

```bash
# Crea una nueva rama y cambia inmediatamente a ella.
git checkout -b 01-custom-skill

# Agrega todos los cambios al staging.
git add .

# Crea un commit con los cambios realizados.
git commit -m "OpenCode Skills - Custom Skills"

# Publica la rama y configura su upstream remoto.
git push -u origin 01-custom-skill
```

GitHub mostrará la posibilidad de crear un Pull Request:

![Comparación para crear el Pull Request](../assets/09-pull-request-compare.png)

Seleccionar:

```text
Compare & pull request
```

En `Open a pull request`:

1. Verificar que aparezca `Able to merge`.
2. Agregar una descripción si es necesario.
3. Seleccionar `Create pull request`.

> **NOTA:** si el repositorio requiere aprobación, esta puede ser realizada por otro miembro del equipo o por el mecanismo de revisión definido para el proyecto.

![Pull Request listo para fusionar](../assets/10-pull-request-merge.png)

Después de aprobar los cambios:

```text
Merge pull request
```

y posteriormente:

```text
Confirm merge
```

![Confirmación de la fusión del Pull Request](../assets/11-confirm-merge.png)

Finalmente seleccionar:

```text
Delete branch
```

---

## 4. Actualizar el repositorio local

Después del merge:

```bash
# Cambia nuevamente a la rama principal.
git checkout main

# Descarga e integra los cambios aprobados desde el remoto.
git pull

# Elimina la rama local que ya fue integrada.
git branch -d 01-custom-skill
```

---

[← Anterior](04-preparar-opencode-para-sdd.md) | [Temario](../09-opencode-spec-driven-development.md) | [Siguiente →](06-crear-generar-y-aprobar-una-spec.md)
