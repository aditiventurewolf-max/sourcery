# Sourcery

A vendor sourcing tool. Describe a project, get vendors ranked against it, and send outreach from one screen.

## What it does

1. **Parse requirements.** Paste a project description and raw requirements. Claude returns a list of criteria, each tagged high, medium or low.
2. **Score vendors.** Every vendor in the database gets a 0 to 100 fit score against those criteria, with a one-sentence reason. High criteria count for more.
3. **Draft outreach.** Claude writes a short email (under 150 words) to a chosen vendor, based on that vendor's profile, asking for a 20-minute call.
4. **Send and track.** Emails go out through Gmail. Each outreach moves through draft, sent, replied, shortlisted and rejected, and a status board shows where each vendor stands.
5. **Follow up.** A job runs daily at 9:00 AM. For vendors who have not replied it drafts and sends a follow-up, up to three, then marks the outreach rejected.

## Stack

- Client: React, Vite, Tailwind
- Server: Node, Express, better-sqlite3
- AI: Anthropic SDK (model set in `server/services/ai.js`)
- Email: Nodemailer over Gmail
- Scheduling: node-cron

## Run it

```bash
npm run install:all
# create a .env with the variables below
npm run dev
```

Environment variables used in the code: `ANTHROPIC_API_KEY`, `GMAIL_USER`, `GMAIL_APP_PASSWORD` (use a Gmail app password, not your account password). The server listens on port 3001 unless `PORT` is set.

The database starts with sample vendors if the vendors table is empty.

## Limitations

- Model output goes straight into `JSON.parse` with no validation or retry, so a malformed reply throws.
- The follow-up job sends real email with no human check. Review before enabling it.
- No tests and no auth. Built as a single-user tool.
- No measurement yet of how good the scores or emails are.
