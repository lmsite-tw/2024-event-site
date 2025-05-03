# 節目單系統 Open Source

這個系統使用 NitroJS 存取 存取 Supabase / Postgres 的資料庫的學生資料

## API
### Supabase
```typescript
import { createClient } from '@supabase/supabase-js'
const supabaseURL = process.env.SUPABASE_URL
const supabasetoken = process.env.SUPABASE_KEY
const supabase = createClient(`${supabaseURL}`, `${supabasetoken}`)
export default defineEventHandler(async (event) => {
        try {
            const { data } = await supabase.from('SOURCETABLE').select()
            return data;
        } catch (error) {
            console.log('error', error);
            return {
                error: 500
            }
        }
    }
});
```
### Postgres
#### postgres.ts
```typescript
import postgres from 'postgres'

const sql = postgres(process.env.POSTGRES_URL, {})

export default sql
```
#### 主要.ts
```typescript
import sql from '~/postgres.ts'
export default defineEventHandler(async (event) => {
        try {
            const data = await sql`
                SELECT * FROM SOURCETABLE;
            `
            return data;
        } catch (error) {
            console.log('error', error);
            return {
                error: 500
            }
        }
    }
});
```
