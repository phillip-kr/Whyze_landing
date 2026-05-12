# Whyze Landing

Static Whyze landing page with a Supabase-backed beta waitlist form.

## Supabase Setup

Use only the public anon key in `index.html`. Do not place a `service_role` key in browser code.

Run this SQL in the Supabase SQL editor before enabling the form in production:

```sql
create table if not exists public.waitlist (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  name text not null,
  email text not null,
  interest text,
  message text,
  source text not null default 'whyze_landing'
);

alter table public.waitlist enable row level security;

create policy "Allow public waitlist insert"
on public.waitlist
for insert
to anon
with check (true);
```

No public select policy is required. The browser can insert waitlist rows, but it cannot read the submitted list.
