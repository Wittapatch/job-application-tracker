# Job Application Tracker

Track every job or internship you apply for, its status, and when to follow up.
Built with Vue 3, Vite, Tailwind CSS and Supabase.

## Features so far

- Log an application with a validated form.
- Applications are saved to Supabase and listed as cards.
- Each browser signs in anonymously, and Row Level Security keeps each user's applications private.

## Database

| Table | Role |
|---|---|
| `applications` | Primary table: one row per job application, inserted by the Vue form. |
| `application_events` | Supporting table: follow-up events linked to an application by `application_id`. Filled by Make.com. |

Both tables and their Row Level Security policies are created on the Supabase website (SQL Editor and Table Editor), following the course's Supabase manual. They are not stored in this repository.

## Run locally

1. Requires Node 22.18+ or 24.12+.
2. `npm install`
3. Copy `.env.example` to `.env.local` and add your Supabase project URL and publishable key.
4. On the Supabase website, create the `applications` and `application_events` tables and their policies, and turn on Authentication → Sign In / Providers → Allow anonymous sign-ins.
5. `npm run dev`
