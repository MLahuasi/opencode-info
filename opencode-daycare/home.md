## CREAR HOME

1. **CREAR SPEC**

- Se va a modificar el `Home` en base a la `referencia` [`feed.dc.html`](../open-daycare/references/screens/feed.dc.html)
- En modod `Plan` especificar:

```text
/Plan

Implementar el Home `/` basado en `@references/screens/feed.dc.html`, respetando `AGENTS.md` y manteniendo el estilo y efectos visuales de la plantilla. No implementar autenticación, base de datos ni navegación real entre rutas.
```

---

2. **IMPLEMENTAR SPEC**

- Solicitar crear `spec` en modo `Build`

```text
crea spec
```

- Se creó `01-home-feed.md`
- Solicitar implementar `spec` creado:

```
/spec-impl @specs/01-home-feed.md
```

- `Plan` especifica los siguientes pasos

```md
# Todos

1. Update app/layout.tsx with fonts, locale, and metadata.
2. Add global visual tokens/styles in app/globals.css.
3. Create reusable presentational Button.
4. Create responsive StaffSidebar.
5. Add feed types and static mock data.
6. Add reusable post-card variants and inline icons.
7. Add feed content/composer/post list and feature barrels.
8. Compose sidebar and feed in app/page.tsx.
```

---

3. **CRITERIOS DE ACEPTACIÓN**

En [](../open-daycare/specs/01-home-feed.md) se especificaron los siguientes:

```md
### Acceptance criteria

- [ ] `/` carga sin errores de servidor, consola o hidratación.
- [ ] La página muestra sidebar de 248px y feed con ancho máximo de 760px en escritorio.
- [ ] La página muestra barra de navegación inferior en una pantalla móvil.
- [ ] El contenido visible coincide con los datos mock de la plantilla: Caro Giménez, Sala Soles, 12 niños, martes 17 jun y tres publicaciones.
- [ ] Los corazones y contadores usan el tratamiento coral definido en el HTML de referencia.
- [ ] Las fuentes Fredoka y Nunito se cargan mediante `next/font/google`.
- [ ] Los botones y enlaces visuales tienen estados `hover`, `active` y `focus-visible` sin handlers ficticios.
- [ ] El feed contiene landmarks semánticos, un único H1, `aria-current` para Feed y nombre accesible para el control de cierre de sesión.
- [ ] No se implementan autenticación, base de datos, persistencia ni rutas adicionales.
- [ ] `npx eslint app` termina correctamente.
- [ ] `npx tsc --noEmit --incremental false` termina correctamente.
- [ ] `npm run build` termina correctamente.
- [ ] Las capturas de verificación se almacenan bajo `.playwright-mcp/Home/`.
```

**NOTA**: se realiza en este caso mediante [`Un Agente Personalizado - Criterios de Aceptación`](./11-opencode-custom-agent.md)

**IMPORTANTE**:

- Una vez que se aprobaron los criterios de aceptación si es necesario se debe actualizar `AGENTS.md` con la nueva información y versionar en la rama.
- Una vez sin cambios por versionar se debe realizar un `merge` a la rama `main`:

```bash
git switch main
git merge spec-01-home-feed
```
