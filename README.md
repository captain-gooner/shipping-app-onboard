# ERP Spec Questionnaire

Static single-page form. Saves responses to Supabase. No build step, no server.

## 1. Supabase setup (5 min)

1. Create free project at supabase.com.
2. SQL Editor → run:

```sql
create table erp_spec_responses (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  company_name text,
  contact_email text,
  answers jsonb not null
);

alter table erp_spec_responses enable row level security;

-- Public can INSERT only. Nobody reads without service key / dashboard.
create policy "public insert" on erp_spec_responses
  for insert to anon with check (true);
```

3. Project Settings → API → copy `Project URL` + `anon public` key.
4. Paste both into `index.html` (`SUPABASE_URL`, `SUPABASE_ANON_KEY`). Anon key safe to expose — RLS blocks reads.

## 2. Deploy

Pick one:

- **GitHub Pages** (simplest): push repo → Settings → Pages → deploy from branch. Done.
- **Vercel / Netlify**: import repo, zero config.
- **Lovable**: connect GitHub repo in Lovable → publish. Works, but Lovable adds value only if you want to keep editing app via prompts there.

## 3. Read responses

Supabase dashboard → Table Editor → `erp_spec_responses`. `answers` column = full JSON per submission. Export CSV from dashboard.

Optional email alert per submission: Supabase Database Webhook → Zapier/Resend.
