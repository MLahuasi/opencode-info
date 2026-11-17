# References

Añadir directorios locales y repositorios Git como referencias de proyectos

Consulta la [documentación oficial de referencias](https://opencode.ai/docs/references/) para conocer las opciones disponibles.

---

## 1. Directorios locales

Añadir en [opencode.jsonc](../open-daycare/opencode.json) `path` para referenciar un directorio local.

```json
{
  "references": {
    "docs": {
      "path": "../docs"
    }
  }
}
```

Desde `OpenCode` se puede llamar al directorio `docs` que esta fuera del proyecto y solicitar informacion

```powershell
@docs que contiene este directorio?
```

```md
El directorio contiene un único archivo: open-daycare-schema.md. Es un diccionario de datos objetivo para PostgreSQL/Supabase, no una implementación ejecutable.
Qué define

- 14 tablas activas:
  daycares, users, credentials, rooms, children, parent_children, invitations, staff_rooms, posts, post_children, post_media, reactions, comments y feed_overviews.
- Enums de dominio para roles, estados, tipos de publicaciones, relaciones familiares y formatos multimedia.
- Tipos de datos, campos obligatorios, valores por defecto, claves primarias, claves foráneas, unicidad y acciones ON DELETE.
- Separación multi-tenant mediante daycare_id.
- Reglas de seguridad y futuras políticas RLS.
- Índices recomendados.
- Mapeo entre las colecciones JSON actuales y las futuras tablas SQL.
  Decisiones importantes
- La persistencia JSON sigue siendo la fuente usada por el runtime actual.
- No se crean migraciones, seeds, repositorios PostgreSQL ni adaptadores Supabase.
- No se conservan necesariamente los IDs legacy; las entidades usarán UUID.
- La autenticación seguirá siendo propia. credentials no se vincula a auth.users.
- daily_summaries y devices quedan fuera del alcance actual.
- credentials.password_hash solo debería ser accesible por el backend privilegiado.
  Reglas de integridad destacadas
- Todas las relaciones deben pertenecer a la misma guardería.
- Un post debe tener exactamente un destino: una sala o uno o más niños.
- Un post puede tener como máximo cuatro archivos multimedia.
- Los comentarios deben tener entre 1 y 500 caracteres normalizados.
- Una reacción por usuario y publicación.
- Las invitaciones deben mantener consistencia entre status, accepted_at, accepted_user_id y expires_at.
- Solo personal autorizado puede acceder a determinadas salas, niños y publicaciones.
  Estado actual
  El archivo sirve como contrato de diseño para una futura migración a PostgreSQL/Supabase. No modifica la aplicación ni los JSON existentes. Para implementarlo después todavía harían falta:

1. Migraciones SQL.
2. Estrategia de migración de IDs y datos JSON.
3. Repositorios/adaptadores PostgreSQL.
4. Políticas RLS concretas.
5. Triggers o procedimientos para las reglas que cruzan varias tablas.
6. Integración con la autenticación propia.
```
