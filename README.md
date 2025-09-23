# Supabase + React + TypeScript + Vite

## Connect to Supabase

### Command to install Supabase React Auth helper:

```bash
yarn add @supabase/auth-ui-react
yarn add @supabase/supabase-js
```

> **Note:** The anon key is safe to use on the frontend as it only allows authorized operations. The service key should only be used on the backend and kept secure.

### supa-client.ts

```typescript
import { createClient } from "@supabase/supabase-js";

const supabaseUrl = (import.meta as any).env.VITE_SUPABASE_API_URL;
const supabaseKey = (import.meta as any).env.VITE_SUPABASE_ANON_KEY; // Safe for frontend

export const supaClient = createClient(supabaseUrl, supabaseKey);
```

## Database Tables

We need to create table in starting ddl as we dont want our tables to disappeared for every time we start and stop supabase local.

```sql
create table user_profiles (
  user_id uuid primary key references auth.users (id) not null,
  username text unique not null
  CONSTRAINT proper_username CHECK (username ~* '^[a-zA-Z0-9_]+$')
  CONSTRAINT username_length CHECK (char_length(username) > 3 and char_length(username) < 15)
);
```

## Enable Row Level Security

```sql
alter table <table name> enable row level security
```

## Add Policy

```sql
CREATE POLICY "all can see" ON "public"."user_profiles"
AS PERMISSIVE FOR SELECT
TO public
USING (true);

CREATE POLICY "users can insert" ON "public"."user_profiles"
AS PERMISSIVE FOR INSERT
TO public
WITH CHECK (auth.uid() = user_id);

CREATE POLICY "owners can update" ON "public"."user_profiles"
AS PERMISSIVE FOR UPDATE
TO public
USING (auth.uid()=user_id)
WITH CHECK (auth.uid()=user_id);
```

## Running Migrations

When you run `supabase stop` and `supabase start`, migrations don't run automatically. To apply migrations:

```bash
# Reset database and apply all migrations
npx supabase db reset

# Or apply pending migrations only
npx supabase migration up
```

> **Note:** `supabase stop/start` preserves database state. Use `db reset` or `migration up` to run your SQL migrations.

## Generate database type for frontend

```bash
npx supabase gen types typescript --local > ./src/database.types.ts
```


