---
name: db-migration-checker
description: Validates database migration files for safety issues before they run in production. Checks for destructive operations, missing rollbacks, unsafe index creation, and transaction wrapping. Invoke before merging any PR that includes migrations.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: project
allowed-tools: Bash Read Glob
---

# Database Migration Checker

Validate all migration files in the current branch before they reach production.

## Step 1 — Detect the migration setup

```bash
# Find migration files changed in this branch
BASE=$(git remote show origin 2>/dev/null | grep 'HEAD branch' | awk '{print $NF}')
BASE=${BASE:-main}
git diff --name-only $(git merge-base HEAD $BASE)...HEAD | grep -iE 'migrat|schema|sql' | head -20
```

Also detect the ORM/framework:
```bash
cat package.json 2>/dev/null | grep -iE '"knex"|"sequelize"|"prisma"|"typeorm"|"drizzle"'
cat requirements.txt pyproject.toml 2>/dev/null | grep -iE 'alembic|django|sqlalchemy|peewee'
cat Gemfile 2>/dev/null | grep -iE 'activerecord|sequel'
ls prisma/migrations db/migrate migrations/ 2>/dev/null
```

Read 1–2 existing migration files to understand the project's migration format before analyzing new ones.

## Step 2 — Read all new/modified migration files

For each migration file changed in this branch, read it fully.

## Step 3 — Apply each safety check

### Check 1: Destructive operations
Flag any of the following without a confirmed justification:
- `DROP TABLE`, `DROP COLUMN`, `DROP INDEX`, `DROP CONSTRAINT`
- `TRUNCATE`
- Column type changes that reduce capacity (e.g., `TEXT` → `VARCHAR(50)`, `BIGINT` → `INT`)
- Removing NOT NULL constraints from columns with existing data
- Renaming tables or columns (breaks running code that uses the old name)

### Check 2: Rollback / down migration
Check whether the migration has a rollback path:
- Does it have a `down()` / `revert()` / `rollback()` function?
- Is the rollback meaningful (not empty, not a no-op)?
- Does the rollback actually reverse the `up()` operation correctly?

Flag if rollback is missing or incorrect.

### Check 3: Index creation safety
Large table index creation without `CONCURRENTLY` (PostgreSQL) or equivalent will lock the table:
```sql
-- Unsafe (locks table):
CREATE INDEX idx_users_email ON users(email);

-- Safe (PostgreSQL):
CREATE INDEX CONCURRENTLY idx_users_email ON users(email);
```

Flag `CREATE INDEX` without `CONCURRENTLY` on tables that are likely to have significant data.

### Check 4: Transaction wrapping
- Is the migration wrapped in a transaction?
- Are there operations that cannot run inside a transaction (e.g., `CREATE INDEX CONCURRENTLY` in PostgreSQL, `ALTER TABLE` in some DBs)?
- If mixing transactional and non-transactional operations, flag it.

### Check 5: Default values on existing columns
Adding a NOT NULL column without a default to an existing table will fail or lock:
```sql
-- Unsafe on non-empty table:
ALTER TABLE users ADD COLUMN verified BOOLEAN NOT NULL;

-- Safe:
ALTER TABLE users ADD COLUMN verified BOOLEAN NOT NULL DEFAULT false;
```

### Check 6: Data migrations mixed with schema migrations
Flag if a migration both modifies schema AND migrates data. These should be separate migrations for safety.

### Check 7: Ordering and dependencies
If multiple migrations are added in this branch, check that they don't have ordering conflicts or depend on each other incorrectly.

## Step 4 — Produce the report

```
## Migration Safety Report

Migrations reviewed: <list file names>
Database: <detected DB type if possible>

### Blockers — Do not run in production
[🚨 Issue | File | Line | Detail | Recommended fix]

### Warnings — Review before running
[⚠️  Issue | File | Line | Detail | Recommended fix]

### Passed
[✅ Check name]

---
Verdict: [✅ Safe to run | ⚠️ Review recommended | 🚨 Do not run]
```
