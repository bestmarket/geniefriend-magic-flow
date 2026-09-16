# Import genie-buddy-fast from GitHub

## What this is

`github.com/bestmarket/genie-buddy-fast` is a public repository containing a complete Lovable app ("Genie Buddy") on the same TanStack Start stack as this project. It's an AI video-creation assistant with:

- Sign-in/sign-up and per-user data (profiles, projects, sources, analyses, scheduled videos)
- Pages: app dashboard, chat (AI brainstorm), channels, sources, studio (video editor)
- AI analysis of YouTube videos/transcripts, browser-side video assembly
- A scheduled-videos webhook endpoint
- Database schema in `drizzle/migrations/` (two SQL migrations with RLS policies and grants already included)

This project is currently the blank starter template, so the import replaces the placeholder with the full app.

## Steps

1. **Copy the code** — clone the repo to a temp folder and copy into this project:
   - `src/` (routes, components, lib, integrations, hooks, styles)
   - `drizzle/`, `drizzle.config.ts`, `package.json` dependencies, config files
   - Skip: `.git`, `.env` (secrets are re-created by Lovable Cloud), `.lovable/`, `node_modules`, lockfile conflicts
2. **Install any missing dependencies** from the imported `package.json`.
3. **Enable Lovable Cloud** — provides login, database, and server-side secrets the app needs (it was built against the same managed backend, so the env var names match).
4. **Apply the database schema** — run both migration SQL files (`0000_import_channel_genie_schema.sql`, `0001_add_video_style.sql`) against this project's database.
5. **Verify** — build passes, home page loads, sign-up/sign-in works, and one create/read flow (e.g. create a project, add a source) succeeds.

## Notes

- AI features use the built-in AI gateway already wired in the imported code; no extra keys needed.
- The browser-based video assembly works as-is; true server-side video rendering is a known limitation of the source app (documented in its roadmap) and stays as-is.
- After import, the app can optionally be connected to your own GitHub repo for two-way sync.

## Technical details

- Source: public repo, HEAD commit `3c4d147`
- Tables created: `profiles`, `projects`, `sources`, plus remaining tables in the two migrations — all with `GRANT`s, RLS enabled, and per-user policies (`auth.uid() = user_id`)
- Auth uses the managed `_authenticated` route gate; public webhook lives under `/api/public/hooks/scheduled-videos`
