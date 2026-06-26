# Fumble Wall — setup (5 min, free)

## 1. Make a Supabase project
- Go to https://supabase.com → New project (free tier). Wait for it to spin up.

## 2. Create the table — run this in the SQL Editor
```sql
create table fumbles (
  id bigint generated always as identity primary key,
  created_at timestamptz default now(),
  who text not null,
  what text,
  media_path text,
  media_type text
);

-- open access: anyone can read + add (no login). Friends-only by obscurity.
alter table fumbles enable row level security;
create policy "anyone read"   on fumbles for select using (true);
create policy "anyone insert" on fumbles for insert with check (true);
```

## 3. Create the storage bucket
- Storage → New bucket → name it exactly `evidence` → toggle **Public** ON → create.
- Then Storage → Policies → on `evidence`, add a policy allowing `insert` and `select` for everyone
  (template: "Allow access to all users", apply to both SELECT and INSERT).

## 4. Paste your keys
- Settings → API → copy **Project URL** and **anon public** key.
- `cp config.example.js config.js`, then put the two values in `config.js`.
- `config.js` is gitignored, so keys never get committed.

## 5. Host it (so friends can open it)
- Drag the folder onto https://app.netlify.com/drop  (or Vercel, or GitHub Pages). Done — share the link.
- Opening the file locally works too, but only for you.

That's it. Everyone hitting the link sees + adds to the same wall.
```
```
