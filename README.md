# AMS Tracker

A ticket triage tracker for an application support team, built on **Google Apps Script** with a **Google Sheet** as its database.
The page (`index.html`) can run inside Apps Script or from GitHub Pages; set `API_URL` in `index.html` to the web app's `/exec` link.

- Pulls tickets from an AIO Reports (Jira) OData export on a schedule
- Team triage: actual status, team, assignee, priority, hours, remarks with history
- Queues, dashboard, reports (print / CSV), who's online, PIN sign-in
- Outlook email composer (sent from the user's own Outlook)

**This repository contains code only.** No ticket data, passwords or tokens are stored here.
All data lives in the Google Sheet; all secrets live in the script's **Script Properties**.

## Files

| File | Purpose |
|---|---|
| `sync.gs` | Scheduled sync from AIO into the `Tickets` sheet |
| `app.gs` | Web app server: tickets, triage, remarks, reports, import |
| `auth.gs` | PIN sign-in and sessions |
| `presence.gs` | Who's online |
| `notify.gs` | Email builders (Outlook compose; optional automatic Gmail sending) |
| `api.gs` | JSON endpoint so the page can be hosted on GitHub Pages |
| `index.html` | The web app page |
| `appsscript.json` | Apps Script manifest |

## Script Properties (set in Apps Script → Project Settings — never in code)

| Property | Value |
|---|---|
| `AIO_URL` | AIO OData export URL ending in `/Issues` (contains an access token – keep private) |
| `AIO_USER` / `AIO_PASS` | AIO account |
| `SHEET_ID` | ID of the database Google Sheet |
| `JIRA_BASE_URL` | Jira portal link prefix, e.g. `https://<site>.atlassian.net/servicedesk/customer/portal/<n>/` |
| `AIO_HOURS_SHIFT` | Optional. Hours to correct AIO timestamps |
| `AUTO_EMAIL` | Optional. `on` = send emails automatically from the owner's Google account |

## Working on the code

```bash
clasp pull                      # get the latest from Apps Script
# edit files
clasp push                      # upload to Apps Script
git add . && git commit -m "…"  # save to GitHub
git push
clasp deploy -i <DEPLOYMENT_ID> -d "short description"   # publish, same web app link
```

## Rules

- Never commit exports, CSVs, screenshots or anything containing ticket data.
- Never put passwords, tokens or URLs with tokens in code – use Script Properties.
- Keep this repository **private**.
