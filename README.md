# Vercel + Supabase Guestbook (free, public demo)

A minimal public guestbook:

- **Vercel** hosts the Next.js app (pages + a serverless API route at `/api/stats`).
- **Supabase** stores messages in Postgres, protects the table with Row Level Security, and pushes new messages to every open browser via Realtime.

Both services have free tiers that need **no credit card** (Vercel Hobby, Supabase Free). The app uses only the public anon key; there's no login and no personal data.

## 1. Set up Supabase (about 3 minutes)

1. Sign up at https://supabase.com with GitHub or email, then click **New project**. Choose any name and region and generate a database password (you won't need it for this app).
2. Open **SQL Editor > New query**, paste the contents of `supabase/schema.sql`, and click **Run**. This creates the `messages` table, the public read/insert policies, Realtime, and two sample rows.
3. Open **Project Settings > API**, or the **Connect** button, and copy:
   - **Project URL** → `NEXT_PUBLIC_SUPABASE_URL`
   - **anon / publishable key** → `NEXT_PUBLIC_SUPABASE_ANON_KEY`

   Never use the `service_role` / secret key in this app.

## 2. Push the code to GitHub

Create a public repo and push this folder:

```bash
git init && git add . && git commit -m "Vercel + Supabase guestbook"
git branch -M main
git remote add origin https://github.com/<you>/vercel-supabase-guestbook.git
git push -u origin main
```

## 3. Deploy on Vercel (about 2 minutes)

1. Sign up at https://vercel.com with GitHub and select the **Hobby** (free) plan.
2. Click **Add New > Project**, import the repo, and keep the Next.js defaults.
3. Under **Environment Variables**, add the two `NEXT_PUBLIC_…` values from step 1.3.
4. Click **Deploy**. You get a public URL like `https://vercel-supabase-guestbook.vercel.app`.

Every later `git push` to `main` redeploys automatically, and pull requests get preview URLs.

## Run locally (optional)

```bash
cp .env.example .env.local   # fill in the two values
npm install
npm run dev                  # http://localhost:3000
```

## What to show in a demo

| Feature | Where |
|---|---|
| Read and write from the browser with RLS | `app/Guestbook.js`, `supabase/schema.sql` |
| Live updates: post in one tab and it appears in another | Realtime subscription in `Guestbook.js` |
| Serverless function on Vercel reading Supabase | `app/api/stats/route.js` → `/api/stats` |
| Vercel region badge | `app/page.js` (`VERCEL_REGION`) |
| Database constraints (name ≤ 40 chars, message ≤ 280 chars) | `check` constraints in `schema.sql` |

## Clean up

Delete the Vercel project (**Settings > Advanced > Delete**) and the Supabase project (**Settings > General > Delete project**). Free Supabase projects also pause after about a week without activity.
