# SplitSave

A simple, mobile-first shared-expenses and savings app implemented from the supplied SplitSave build specification.

## Stack

- Next.js 16.3.8 / App Router
- React 19.3
- TypeScript 7
- Supabase Auth + Postgres + Row Level Security
- Plain CSS (no UI framework dependency)
- Vitest for pure financial/gamification logic

The project uses the current Supabase SSR approach with `@supabase/ssr`, cookie-based sessions, and a Next.js `proxy.ts` for auth refresh. citeturn522971search3turn522971search4

## What is implemented

### P0
- Email/password authentication
- Groups and invite links
- Equal / exact / percentage expense splits
- Integer minor-unit money storage
- Balances and simplified settle-up suggestions
- Manual settlement records
- Savings goals and contributions
- Opt-in leaderboard using progress % and streak only
- Server-side authorization through Supabase RLS

### P1
- Personal expenses and monthly category totals
- Weekly streaks
- Points
- Badges derived from source records
- Activity feed

### Explicitly excluded from v1
- real payments
- bank linking
- multi-currency conversion
- receipt OCR
- push notifications
- native mobile apps

## 1. Create Supabase

Create a new Supabase project. Current Supabase docs recommend the Next.js SSR setup with `@supabase/ssr` and the project's URL + publishable key. citeturn522971search0turn522971search3

In Supabase SQL Editor, run:

```text
supabase/migrations/001_init.sql
```

This creates the schema, trigger, RLS policies, and secure RPCs.

## 2. Configure environment variables

Copy:

```bash
cp .env.example .env.local
```

Then set:

```text
NEXT_PUBLIC_SUPABASE_URL=https://YOUR-PROJECT.supabase.co
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=YOUR_SUPABASE_PUBLISHABLE_KEY
```

Do not use a service-role key in the browser and do not commit `.env.local`.

## 3. Run locally

Requirements: Node.js 20.9+.

```bash
npm install
npm run dev
```

Open http://localhost:3000.

Run unit tests:

```bash
npm test
```

Run a production build:

```bash
npm run build
```

## 4. Deploy to Vercel

1. Push this folder to GitHub.
2. Import the repository into Vercel.
3. Add the same two Supabase environment variables in Vercel Project Settings → Environment Variables.
4. Deploy.
5. In Supabase Auth settings, add your Vercel deployment URL to the allowed redirect/site URLs as appropriate for your authentication configuration.

Vercel supports Next.js deployments directly; keep the project on the standard Next.js build command (`next build`).

## Important implementation notes

- Money is stored as integer minor units (`amount_minor`).
- Shared-expense writes use security-definer Postgres RPCs that re-check authenticated group membership and share totals.
- User privacy is enforced in Postgres RLS, not only in UI conditionals.
- Personal expenses and savings goals are private to their owner.
- The leaderboard RPC returns progress/streak fields rather than absolute savings amounts.
- The leaderboard treats a user's progress across goals as aggregate saved amount / aggregate target, capped at 100%. This is an implementation choice because the source specification permits multiple goals but does not define how multiple goals are aggregated for ranking.
- The first version intentionally avoids a payment rail; settlements are records only.

## Project structure

```text
app/
  auth/
  dashboard/
  groups/
  goals/
  personal/
  invite/
components/
lib/
supabase/
  migrations/
tests/
```

## Demo data

`supabase/seed_demo.sql` is a safe starting point for creating demo records after you have created three real Supabase Auth users. It intentionally does not fabricate `auth.users` rows.

## Source specification

`SOURCE_BUILD_SPEC.txt` contains the original supplied build specification used as the implementation basis.
