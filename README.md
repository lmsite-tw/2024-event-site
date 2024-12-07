# 節目單系統 Open Source

這個系統使用 yuanhau.com(創作者的網站) 的 API 來取得資料。

### API
```typescript
import { createClient } from '@supabase/supabase-js'
const suoabaseURL = process.env.SUPABASE_URL
const supabasetoken = process.env.SUPABASE_KEY
const supabase = createClient(`${supabaseURL}`, `${supabasetoken}`)
export default defineEventHandler(async (event) => {
    if (event.node.req.method === 'POST') {
        try {
            const { data } = await supabase.from('aiyuewu2024shengdanyinyuehui').select()
            return data;
        } catch (error) {
            console.log('error', error);
            return {
                error: 500
            }
        }
    } else {
        return {
            error: 403
        }
    }
});
```
