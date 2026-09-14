# Signal & Frame — studio site

A single-page static site (no build step needed) for a video editing / web design /
marketing / consulting studio. The contact form saves leads directly into Supabase.

## 1. Set up Supabase

1. Go to https://supabase.com, sign up free, and create a new project.
2. Once it's ready, open **SQL Editor** and run this to create the leads table:

```sql
create table leads (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  company text,
  email text not null,
  service text not null,
  message text not null,
  created_at timestamp with time zone default now()
);

-- Allow anyone (anonymous visitors) to submit a lead, but not read/edit/delete leads.
alter table leads enable row level security;

create policy "Anyone can submit a lead"
  on leads for insert
  to anon
  with check (true);
```

This keeps the table locked down: visitors can only add new rows, never read, edit,
or delete existing ones. You'll view submissions yourself inside the Supabase
dashboard (Table Editor → leads).

3. Go to **Project Settings → API** and copy:
   - Project URL
   - `anon` public key

The anon key is meant to be public (it ships in your website's code) — the RLS
policy above is what actually protects your data, not keeping this key secret.

## 2. Connect the site to Supabase

Open `index.html`, find this block near the bottom, and paste your values in:

```js
const SUPABASE_URL = "YOUR_SUPABASE_URL";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
```

## 3. Push to GitHub

```bash
cd site
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

(Create the empty repo on github.com first, then run the commands above.)

## 4. Deploy for free (Vercel or Netlify)

**Vercel**
1. Go to vercel.com → sign in with GitHub → "Add New Project"
2. Select your repo → since this is a plain static site, leave the build settings
   default (no framework, no build command needed) → Deploy
3. You'll get a live URL like `your-project.vercel.app`

**Netlify** works the same way — "Add new site" → "Import an existing project" →
pick your GitHub repo → deploy with no build command.

Every time you `git push` a change, your live site updates automatically.

## 5. Before showing this to real clients

- Replace the email, WhatsApp number, and "Based in" line in the Contact section
  with your real details.
- Replace the "Recent work" section with actual screenshots or video once you've
  completed your first 1–3 projects. Leaving it as placeholder text is fine short
  term — inventing fake past clients or testimonials is not, and is the fastest way
  to lose a serious client's trust if they check.
- Consider adding a custom domain (e.g. signalandframe.com) once you've picked a
  real studio name — Vercel and Netlify both support this for a small yearly fee
  from a domain registrar.
