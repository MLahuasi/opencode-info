# Supabase database schema

## Purpose

This document is the source of truth for the future relational model of Open Daycare in Supabase.

All application tables belong to the PostgreSQL `public` schema. The modules below are logical documentation groups, not PostgreSQL schemas.

## Modules

- **global:** `daycares`
- **auth:** `people`, `credential`, `invitation`
- **rooms:** `rooms`, `staff_rooms`
- **children:** `kids`, `parent_kids`
- **social-media:** `feed`, `feed_kids`, `feed_media`, `feed_comments`, `feed_overview`, `feed_reactions`

## Multi-daycare conventions

- Every persisted identifier uses PostgreSQL `uuid`.
- Database-generated identifiers use `gen_random_uuid()` by default.
- Every table includes a required `daycare_id` foreign key to `daycares(id)`.
- Every table includes `created_at timestamptz NOT NULL DEFAULT now()` and `updated_at timestamptz NOT NULL DEFAULT now()`, unless a table-specific definition states otherwise.
- Relationships between records in the same daycare use composite foreign keys containing both the record ID and `daycare_id`.
- Email uniqueness is scoped to a daycare, not globally.
- JSON source IDs are migration-only identifiers and must not be persisted as database IDs.
- The initial JSON snapshot is loaded into one daycare named `Guardería Sala Soles`.

## `global` module

### `daycares`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `name` | `text` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

The initial seed creates the daycare with `name = 'Guardería Sala Soles'`.

## PostgreSQL enums

The schema defines enums from the current domain contracts:

| Enum | Values |
| --- | --- |
| `person_role` | `parent`, `personal` |
| `person_status` | `active`, `inactive`, `pending` |
| `parent_relationship` | `mother`, `father`, `guardian` |
| `kid_status` | `active`, `inactive` |
| `post_type` | `food`, `nap`, `activity`, `achievement`, `mood`, `announcement` |
| `post_reaction_type` | `love` |
| `post_media_format` | `jpg`, `jpeg`, `png`, `webp` |
| `post_media_resource_type` | `image` |
| `post_media_delivery_type` | `authenticated` |

Invitation status is derived from `expires_at` and `accepted_at`; it is not persisted as an enum.

## `auth` module

### `people`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `name` | `text` | NOT NULL |
| `email` | `text` | NOT NULL |
| `role` | `person_role` | NOT NULL |
| `status` | `person_status` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

Create a unique index on `(daycare_id, lower(email))`. The same email may exist in different daycares.

### `credential`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `person_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `password_hash` | `text` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

`credential` stores the existing password hashes and remains separate from Supabase `auth.users`.

### `invitation`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `person_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `kid_id` | `uuid` | NOT NULL, composite FK to `kids(id, daycare_id)` |
| `relationship` | `parent_relationship` | NOT NULL |
| `code` | `text` | NOT NULL |
| `expires_at` | `timestamptz` | NOT NULL |
| `sent_at` | `timestamptz` | NULL |
| `accepted_at` | `timestamptz` | NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

## `rooms` module

### `rooms`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `name` | `text` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

### `staff_rooms`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `person_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `room_id` | `uuid` | NOT NULL, composite FK to `rooms(id, daycare_id)` |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

## `children` module

### `kids`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `slug` | `text` | NOT NULL |
| `name` | `text` | NOT NULL |
| `birth_date` | `date` | NOT NULL |
| `room_id` | `uuid` | NOT NULL, composite FK to `rooms(id, daycare_id)` |
| `enrollment_date` | `date` | NOT NULL |
| `medical_notes` | `text` | NOT NULL |
| `allergies` | `text` | NOT NULL |
| `status` | `kid_status` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

### `parent_kids`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `parent_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `kid_id` | `uuid` | NOT NULL, composite FK to `kids(id, daycare_id)` |
| `relationship` | `parent_relationship` | NOT NULL |
| `photo_sharing_consent` | `boolean` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

## `social-media` module

### `feed`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `type` | `post_type` | NOT NULL |
| `author_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `created_at` | `timestamptz` | NOT NULL |
| `updated_at` | `timestamptz` | NOT NULL |
| `body` | `text` | NOT NULL |
| `room_id` | `uuid` | NULL, composite FK to `rooms(id, daycare_id)` |

The JSON arrays `kidIds` and `media` are not stored directly in `feed`; they are normalized in the auxiliary tables documented in Step 3.

### `feed_comments`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `feed_id` | `uuid` | NOT NULL, composite FK to `feed(id, daycare_id)` |
| `author_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `body` | `text` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL |
| `updated_at` | `timestamptz` | NULL |

`updated_at` remains nullable so `updatedAt: null` from the JSON snapshot is preserved.

### `feed_reactions`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `feed_id` | `uuid` | NOT NULL, composite FK to `feed(id, daycare_id)` |
| `person_id` | `uuid` | NOT NULL, composite FK to `people(id, daycare_id)` |
| `type` | `post_reaction_type` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

### `feed_kids`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `feed_id` | `uuid` | NOT NULL, composite FK to `feed(id, daycare_id)` |
| `kid_id` | `uuid` | NOT NULL, composite FK to `kids(id, daycare_id)` |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

Create `UNIQUE (feed_id, kid_id)` to prevent duplicate recipients in one feed.

### `feed_media`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)` |
| `feed_id` | `uuid` | NOT NULL, composite FK to `feed(id, daycare_id)` |
| `position` | `integer` | NOT NULL |
| `public_id` | `text` | NOT NULL |
| `asset_id` | `text` | NOT NULL |
| `resource_type` | `post_media_resource_type` | NOT NULL |
| `delivery_type` | `post_media_delivery_type` | NOT NULL |
| `format` | `post_media_format` | NOT NULL |
| `width` | `integer` | NOT NULL |
| `height` | `integer` | NOT NULL |
| `bytes` | `integer` | NOT NULL |
| `original_name` | `text` | NOT NULL |
| `alt` | `text` | NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

Create `UNIQUE (feed_id, position)`. The zero-based `position` preserves the order of the source `media` array.

### `feed_overview`

| Column | PostgreSQL type | Null/default/constraint |
| --- | --- | --- |
| `id` | `uuid` | PK, NOT NULL, default `gen_random_uuid()` |
| `daycare_id` | `uuid` | NOT NULL, FK to `daycares(id)`, UNIQUE |
| `room_label` | `text` | NOT NULL |
| `greeting` | `text` | NOT NULL |
| `attendance` | `text` | NOT NULL |
| `date` | `text` | NOT NULL |
| `composer_prompt` | `text` | NOT NULL |
| `published_today_label` | `text` | NOT NULL |
| `created_at` | `timestamptz` | NOT NULL, default `now()` |
| `updated_at` | `timestamptz` | NOT NULL, default `now()` |

`feed_overview` stores the summary information for one daycare. Its unique `daycare_id` enforces one overview row per daycare.

## JSON source mapping

The future seed migration reads exactly these files from `src/infrastructure/persistence/json/data/`:

| JSON source | PostgreSQL destination |
| --- | --- |
| `credential.json` | `credential` |
| `people.json` | `people` |
| `invitation.json` | `invitation` |
| `rooms.json` | `rooms` |
| `staff-rooms.json` | `staff_rooms` |
| `kids.json` | `kids` |
| `parent-kids.json` | `parent_kids` |
| `feed.json` | `feed`, `feed_kids`, `feed_media` |
| `feed-comments.json` | `feed_comments` |
| `feed-overview.json` | `feed_overview` |
| `feed-reactions.json` | `feed_reactions` |

The source JSON files remain unchanged. Their IDs are input keys for migration mapping only.

## JSON-to-UUID migration procedure

The seed process must transform the JSON snapshot into relational rows without changing the source files.

1. Read all 11 JSON collections before inserting dependent records.
2. Insert the daycare `Guardería Sala Soles` and retain its generated UUID as the common `daycare_id`.
3. Generate a new UUID for every source record, including source IDs that already have UUID syntax.
4. Keep one mapping per source collection, for example `people.old_id -> people.new_uuid` and `feed.old_id -> feed.new_uuid`.
5. Resolve every relationship through the corresponding mapping before constructing an insert row.
6. Assign the same daycare UUID to every migrated row.
7. Expand each `feed.kidIds` element into one `feed_kids` row.
8. Expand each `feed.media` element into one `feed_media` row and preserve its zero-based array position.
9. Insert parent records before dependent records so all foreign keys can be validated.
10. Abort the transaction when any referenced source ID cannot be resolved.

The relationship fields that require mapping include:

| Source field | Target mapping |
| --- | --- |
| `credential.personId` | `people` |
| `invitation.personId` | `people` |
| `invitation.kidId` | `kids` |
| `staff-rooms.personId` | `people` |
| `staff-rooms.roomId` | `rooms` |
| `kids.roomId` | `rooms` |
| `parent-kids.parentId` | `people` |
| `parent-kids.kidId` | `kids` |
| `feed.authorId` | `people` |
| `feed.kidIds[]` | `kids` through `feed_kids` |
| `feed.roomId` | `rooms` |
| `feed-comments.postId` | `feed` |
| `feed-comments.authorId` | `people` |
| `feed-reactions.postId` | `feed` |
| `feed-reactions.personId` | `people` |

### Preservation rules

- Preserve dates, bodies, names, emails, hashes, codes, media metadata, booleans and existing `NULL` values.
- Preserve empty strings such as `allergies: ""` and `medicalNotes: ""` as empty strings.
- Preserve `feed-comments.updatedAt: null` as `feed_comments.updated_at = NULL`.
- Use the seed load timestamp only for newly added audit columns when the source record has no equivalent timestamp.
- Do not persist any source ID as a primary key or foreign key.
- Do not rewrite, delete or move any JSON source file.

### Migration verification counts

The current JSON snapshot must produce:

- 11 source collections.
- 115 rows sourced directly from JSON records.
- 1 seeded daycare row.
- 25 `feed_kids` rows generated from feed recipient arrays.
- 11 `feed_media` rows generated from media arrays.
- Zero unresolved or orphaned relationships.

The seed must run in one transaction and must verify these counts and relationship mappings before committing.

## Supabase migration architecture

The future database implementation must use Supabase CLI and versioned migration files. Manual Dashboard changes or one-off MCP DDL are not the source of truth.

Create two ordered files in `supabase/migrations/`:

1. `<timestamp>_create_daycare_schema.sql`
   - Enables required extensions.
   - Creates all PostgreSQL enums.
   - Creates all 14 tables in dependency order.
   - Adds primary keys, composite-key support constraints, indexes and foreign keys.
   - Enables RLS on every `public` table.
   - Does not create client policies.
2. `<timestamp>_seed_daycare_json_snapshot.sql`
   - Inserts `Guardería Sala Soles`.
   - Loads the 11 JSON collections through the UUID mapping procedure.
   - Inserts normalized `feed_kids` and `feed_media` rows.
   - Validates references and expected counts inside one transaction.

The structural migration must run before the seed migration. The deployment workflow is:

```bash
supabase start
supabase migration up
supabase db push
```

The migration files remain committed and reviewable. The future implementation must not apply schema changes directly through Supabase MCP without first recording the equivalent migration file.

## Referential actions and RLS

- Root records `daycares`, `people`, `rooms` and `kids` are protected from cascading deletion through dependent records.
- Child and bridge records may use `ON DELETE CASCADE` from their owning parent when that does not violate root protection.
- Every `public` table enables Row Level Security in the structural migration.
- No `anon` or `authenticated` policies are defined until a future authorization spec defines the identity-to-person relationship.
- The seed and migration process uses administrative access; this does not grant client access.

The feed invariant that a post targets either a room or one or more kids, but never both, remains application validation and is not implemented as a database trigger in this schema document.

## Verification checklist

The future migration implementation is complete only when all of the following checks pass:

- [ ] `src/schemas/schema_db.md` remains the documented source of truth.
- [ ] The structural migration creates exactly 14 application tables in `public`.
- [ ] Every table has a UUID primary key and a required `daycare_id`.
- [ ] Every relationship that crosses tables includes a matching daycare boundary.
- [ ] `people` allows equal emails across daycares but not duplicate lower-cased emails within one daycare.
- [ ] All declared enum values match the domain contracts.
- [ ] RLS is enabled on every application table.
- [ ] No client policy grants access before a future authorization spec defines it.
- [ ] The seed reads exactly the 11 JSON files listed in this document.
- [ ] Every source record receives a newly generated UUID, including records whose source ID already looks like a UUID.
- [ ] No source ID remains as a persisted primary key or foreign key.
- [ ] All source relationships resolve through the generated UUID maps.
- [ ] The seed creates one daycare named `Guardería Sala Soles`.
- [ ] The seed creates 115 rows from JSON records, 25 `feed_kids` rows and 11 `feed_media` rows.
- [ ] The seed reports zero unresolved or orphaned relationships.
- [ ] Empty strings remain empty strings.
- [ ] Existing `NULL` values, including comment `updatedAt`, remain `NULL`.
- [ ] Media order is preserved through zero-based `feed_media.position`.
- [ ] The JSON source files remain byte-for-byte unchanged.
- [ ] `supabase db push` applies the structural migration before the seed migration without manual schema changes.

This document intentionally does not contain executable migration SQL or client authorization policies.

## Implementation boundary

This file documents the target schema only. Future implementation specs must define the SQL migrations, JSON-to-UUID mapping, seed transaction and Supabase CLI deployment separately.


---

[Índice principal](../../../README.md)
