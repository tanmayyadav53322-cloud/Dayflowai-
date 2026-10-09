# DayFlow AI

Plan, focus, complete, learn, improve. Static web app (HTML/CSS/JS, no build step) with Supabase auth and per-user cloud sync, installable as a PWA.

## Structure
```
index.html             App with Supabase auth + cloud sync (PWA-enabled)
manifest.webmanifest   PWA manifest
sw.js                  Service worker (offline app shell)
icon.svg               App icon
standalone/index.html  Same app without Supabase/PWA (data stays in the browser)
supabase/schema.sql    user_state table + Row Level Security
```

## Run locally
`python3 -m http.server 8000` then open http://localhost:8000 (service workers and OAuth need http/https, not file://).

## Supabase setup
1. SQL Editor: run `supabase/schema.sql`.
2. Authentication > URL Configuration: set Site URL and Redirect URLs to your app URL (add http://localhost:8000 for local tests). Password-reset emails return here.
3. Authentication > Providers > Google: enable with a Google Cloud OAuth client; redirect URI `https://ifsuwkcscpxqfjaqfhxc.supabase.co/auth/v1/callback`.
4. Authentication > Providers > Email: choose whether confirmation is required.

The publishable key in `index.html` is safe in the browser because Row Level Security limits each row to its owner. Never commit a secret or service_role key.

## Deploy
Any static host (Netlify, Vercel, Cloudflare Pages, GitHub Pages). No build command; publish the repo root over HTTPS.

## Features
Proposed-plan flow (Accept / Edit / Regenerate), smart rescheduling with preview and undo, execution mode, quick add in natural language, recurring tasks, Day/Week/Month calendar, .ics calendar import, goals with milestones/deadlines/archive, AI Brain (goals, deadlines, habits, learned durations), Ask DayFlow (typed or voice), weekly and monthly insights, XP/achievements, theme (system/dark/light) and accent colour, reminders while the page is open, profile, data export, sign up/login/Google/forgot and change password.

## Not implemented / limitations
- "AI" is rule-based, not a language model. Add one through a server function (e.g. Supabase Edge Function) so keys stay secret.
- Calendar drag-and-drop and resizing, Google/Outlook calendar OAuth, Pomodoro timer presets, billing (Stripe/Razorpay), delete-account, profile picture upload, skeleton loaders and confirmation modals are not built.
- Recurring tasks: edit/delete applies to the whole series only.
- All state is one JSON document per user in `user_state`; normalise into tables before heavy use.
- PWA icon is SVG only; iOS home-screen icons may need a PNG.
- Not tested against the live Supabase project or on real devices.
