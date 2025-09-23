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

