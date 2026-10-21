# Capstone milestone tracker

A static web app: `index.html` plus a `reports/` folder of progress report PDFs.

## Deploy with GitHub Pages
1. Create a GitHub repository and upload everything in this folder (keep the `reports/` folder).
2. Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)` → Save.
3. Your app appears at `https://<your-username>.github.io/<repo-name>/`.

Without Supabase the checklist and notes are saved in each browser separately.

## Optional: sync across phone and computer (Supabase)
1. Create a free project at supabase.com and run this in the SQL Editor:

```sql
create table tracker (
  user_id uuid primary key default auth.uid() references auth.users(id) on delete cascade,
  data jsonb not null default '{}',
  updated_at timestamptz not null default now()
);
alter table tracker enable row level security;
create policy "own row" on tracker for all
  using (auth.uid() = user_id) with check (auth.uid() = user_id);
```

2. Authentication → URL Configuration: set Site URL to your GitHub Pages address.
3. Project Settings → API: copy the project URL and the public (anon/publishable) key into `SUPABASE_URL` and `SUPABASE_KEY` near the top of the script in `index.html`. Never use the secret/service-role key.
4. Commit the change. Open the app, enter your email, and use the sign-in link.
