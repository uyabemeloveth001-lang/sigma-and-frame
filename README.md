# Signal & Frame

A studio site for video editing, web design, marketing, and consulting — built as a single static page with a Supabase-backed contact form.

**Live site:** _add your Vercel URL here once deployed_

---

## About

Signal & Frame presents four services as one connected project pipeline — consulting, web design, video editing, and marketing — so a visitor understands how an engagement actually runs from first call to launch, rather than seeing a disconnected list of skills.

## Tech stack

- **HTML / CSS / vanilla JavaScript** — no build step, no framework, deploys as-is
- **Supabase** — Postgres database + row-level security for storing contact form submissions
- **GitHub** — version control
- **Vercel** — free static hosting with automatic redeploys on push

## Features

- Responsive, single-page layout (desktop, tablet, mobile)
- Services presented as a project pipeline with clear sequencing
- Contact form that writes directly to a Supabase table
- No backend server required — everything runs client-side plus Supabase

## Project structure

```
.
├── index.html      # the entire site: markup, styles, and form logic
└── README.md       # this file
```

## Local setup

1. Clone the repo:
   ```
   git clone https://github.com/uyabemeloveth001-lang/sigma-and-frame.git
   cd sigma-and-frame
   ```
2. Open `index.html` directly in a browser, or use VS Code / VSCodium's **Live Server** extension for auto-reload while editing.

## Connecting the contact form to Supabase

1. Create a free project at [supabase.com](https://supabase.com).
2. In the SQL Editor, run:
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

   alter table leads enable row level security;

   create policy "Anyone can submit a lead"
     on leads for insert
     to anon
     with check (true);
   ```
3. In **Project Settings → API**, copy the **Project URL** and **anon public key**.
4. In `index.html`, find these lines near the bottom and paste your values in:
   ```js
   const SUPABASE_URL = "YOUR_SUPABASE_URL";
   const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
   ```
5. Submissions land in **Table Editor → leads** inside your Supabase dashboard. The row-level security policy above only allows new submissions — visitors can never read, edit, or delete existing leads.

## Deployment

This is a static site, so it deploys with zero configuration:

1. Push this repo to GitHub (already done if you're reading this on GitHub).
2. Go to [vercel.com](https://vercel.com), sign in with GitHub, and import this repository.
3. Leave the framework preset as **Other** with no build command, and deploy.
4. Every `git push` to `main` automatically redeploys the live site.

## Roadmap

- [ ] Replace placeholder contact details (email, WhatsApp, location) with real ones
- [ ] Replace the "Recent work" section with real projects as they're completed
- [ ] Add a custom domain
- [ ] Add analytics to track contact form conversions

## License

© Signal & Frame. All rights reserved.
