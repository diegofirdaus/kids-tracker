# Kids Tracker

Daily task tracker for Diego's two sons, **Danesh** and **Zidane**. Parents add tasks; the boys tick them off on their own phones; parents see a report.

## About Diego (the owner)
- Non-technical product manager, learning to code. Explain changes in plain language, business terms first, no jargon without a short explanation.
- Usually works from an iPad, so avoid steps that need a terminal on his side. Do the code work yourself and give him click-by-click instructions for anything in a web console.
- Writes in English and Bahasa Indonesia.

## Live setup
- **Site:** https://diegofirdaus.github.io/kids-tracker/ (GitHub Pages, branch `main`, root folder). Anything pushed to `main` goes live in 1–2 minutes.
- **Database:** Firebase Firestore, project `kids-tracker-9ea88`, region `asia-southeast2` (Jakarta), Spark (free) plan. The web config is inside `index.html`; it identifies the project and is safe to be public.
- **Access:** anyone with the link can use the app, no login. This is on purpose: the boys don't have accounts.

## Files
- `index.html`: the whole app (HTML, CSS and JavaScript in one file). Loads the Firebase compat SDK v10.12.2 from gstatic.
- `firestore.rules`: a copy of the security rules. **Changing this file does not change the live rules.** Diego has to paste them into Firebase Console → Firestore → Rules → Publish.

## Data model (Firestore)
- `tasks/{id}`: `title`, `kid` (`danesh` | `zidane` | `both`), `repeat` (`daily` | `school` = Mon–Fri | `once`), `date` (YYYY-MM-DD, only for `once`), `created` (YYYY-MM-DD), `createdAt` (ms), `archived` (YYYY-MM-DD or null). "Remove" sets `archived` to today, so past reports stay accurate.
- `checks/{date}_{kid}`: `date`, `kid`, `done` (map of taskId → true), `updatedAt`.
- `settings/main`: `pin` (4-digit string, default `1234`), `reward` (number, default `10000`).

## Product rules
- **Kid mode:** pick a boy, see today's checklist, confetti when everything is done, plus a week strip and reward status.
- **Parent mode:** locked by a 4-digit PIN. It's only a family lock, not real security. Inside: a 7-day report per boy, add or remove tasks, and settings (PIN, reward amount).
- **Weekly reward:** Rp10.000 by default, paid on Sunday, earned only if every day Monday–Sunday is fully done. A day with no tasks counts as done. A missed past day marks the week as missed.
- EN/ID language switch at the top right; Bahasa is the default. Every new text needs both languages (the `T` object in `index.html`).
- Dates use the phone's local time (WIB).

## Working agreements
- Keep it a single static file unless Diego agrees to more structure.
- Before pushing, check the JavaScript for syntax errors. After pushing, tell Diego what changed and how to check it on the live site.
- No surnames, school names or addresses in the app or the repo; the site is public.
- Ideas discussed for version 2: completion time, parent "verified" tick, points or rewards, custom domain, maybe Cloudflare Pages for hosting (keeping Firebase for data).
