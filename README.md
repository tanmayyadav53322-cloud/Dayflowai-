# DayFlow AI

AI-style daily planner: describe your day in plain language, get a realistic schedule, run it step by step, then review and plan tomorrow. Single-file static web app (HTML, CSS, JavaScript), no build step.

## Structure

```
index.html            Main app with Supabase auth + per-user cloud sync
standalone/index.html Same app without Supabase (data stays in the browser)
supabase/schema.sql   user_state table + Row Level Security policies
```

## Run locally

```
python3 -m http.server 8000
# open http://localhost:8000
```

Use a local server (not file://) so Google/email-confirmation redirects work.

## Supabase setup

1. Supabase dashboard > SQL Editor: run `supabase/schema.sql`.
2. Authentication > URL Configuration: set Site URL and Redirect URLs to your app URL (and `http://localhost:8000` for local testing).
3. Authentication > Providers > Google: enable it with a Google Cloud OAuth client. Authorized redirect URI:
   `https://ifsuwkcscpxqfjaqfhxc.supabase.co/auth/v1/callback`
4. Authentication > Providers > Email: choose whether email confirmation is required.

`index.html` contains the project URL and the **publishable** key (safe for the browser because Row Level Security restricts every row to its owner). Never put the secret or service_role key in this repo.

## Deploy

Any static host works: Netlify (drag and drop the folder), Vercel, Cloudflare Pages, or GitHub Pages. No build command; publish directory is the repo root.

## Features

- Natural-language day planning, fixed events, buffers, overload detection
- Execution mode: Complete, Pause, +5 min, Skip, Reschedule, focus rating
- Smart rescheduling with preview and undo; never moves fixed or calendar events
- Calendar: .ics import (stable event IDs, no duplicates) and manual busy time
- AI Brain: goals, deadlines, habits, learned durations, best-window detection
- Goals with milestones, productivity score, XP and achievements
- Ask DayFlow command box (typed or voice) that proposes changes before applying
- Weekly review, patterns, history, next-week plan, reminders (while page is open)
- Supabase email/password and Google sign-in, per-user private cloud storage

## Known limitations

- The "AI" is rule-based (regex and heuristics), not a language model. To add one, call an LLM from a server route (for example a Supabase Edge Function) so the API key stays secret.
- Google Calendar / Outlook OAuth, Razorpay billing, admin panel, analytics and per-feature SEO pages are not implemented; they need a server.
- Whole app state is stored as one JSON document per user (`user_state.state`). Move to normalized tables (tasks, goals, habits, focus_sessions) before heavy use.
- Reminders only fire while the page is open.
- Not yet tested against the live Supabase project.
