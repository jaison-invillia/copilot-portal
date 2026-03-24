---
description: "Use when creating or modifying database migrations. Covers migration patterns, constraints, rollback safety, and schema conventions."
applyTo: "backend/src/infrastructure/db/migrations/**"
---

# Database Migration Guidelines

## Migration standards

- Each migration file represents ONE atomic schema change
- File naming: `YYYYMMDDHHMMSS-<description>.ts` (timestamp prefix)
- Every migration MUST have both `up()` and `down()` methods
- `down()` must fully reverse `up()` — migrations must be rollback-safe

## Schema conventions (per `docs/database.md`)

- **Engine**: InnoDB
- **Charset**: utf8mb4, collation utf8mb4_unicode_ci
- **Primary keys**: `id INT UNSIGNED AUTO_INCREMENT`
- **Audit fields**: `created_at DATETIME DEFAULT CURRENT_TIMESTAMP`, `updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
- **Soft deletes**: use `deleted_at DATETIME NULL` when needed

## Constraints

- Always define foreign keys with explicit `ON DELETE` / `ON UPDATE` behavior
- Use `ON DELETE CASCADE` only when child records should be removed with parent
- Define UNIQUE constraints as documented in `docs/database.md`

## Indexes

- Add indexes for columns used in WHERE, JOIN, ORDER BY
- Foreign key columns are auto-indexed in InnoDB
- Composite indexes: put most selective column first

## Safety rules

- Never DROP a column in the same release as code removal
- Add columns as NULLABLE first, backfill, then add NOT NULL constraint
- Never modify existing constraints in-place — drop and recreate
- Test rollback before considering migration complete
