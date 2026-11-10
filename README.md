# Job Application Tracker

A small Vue 3 app for keeping track of the jobs and internships I apply for. Made for the ISE Vue Frontend Cloud Database assignment.

You fill in a form (company, role, date applied, where you found it, work mode, location, salary, notes) and it saves to Supabase. Your applications show up as cards on the right. Click "View details" to see everything and change the status (Applied, Interview, Offer, Rejected).

## Supabase

Two tables, made on the Supabase website using the SQL Editor like in the course manual:

- `applications`: one row per application, added from the Vue form
- `application_events`: follow-ups for each application, linked by `application_id`

Each browser signs in anonymously, and RLS makes sure you only see your own rows.

## Make.com

When a new row is added to `applications`, a Supabase Database Webhook sends it to Make.com. Make then uses Supabase "Create a Row" to add a `follow_up` row to `application_events`, due 7 days after the applied date. You can see it under Follow-ups in the details page.

## Screenshots

In the `screenshots` folder:

- 01: the form
- 02: the list of applications
- 03, 04: the two Supabase tables
- 06, 07: the Make.com scenario and the Create a Row mapping
- 08: details page with a follow-up from Make
- 09: phone view

## How to run

1. `npm install`
2. Copy `.env.example` to `.env.local` and put in your Supabase URL and publishable key
3. `npm run dev`
