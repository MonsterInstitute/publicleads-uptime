# publicleads-uptime

External uptime monitoring for https://www.publicleads.com.

Public and separate from the application repository for two reasons:

- **Actions minutes are free for public repos.** The private repo's Actions
  were blocked on 2026-09-21 — *"the job was not started because recent
  account payments have failed or your spending limit needs to be increased"* —
  and a monitor that dies with the billing it is meant to outlive is not a
  monitor.
- **It is the only external viewpoint.** The Vercel cron and the laptop
  monitor both watch the site from infrastructure that fails with it. On
  2026-09-20 every URL returned 402 and neither could report it.

## No secrets

Everything here curls a public URL. Alerting is GitHub's own "workflow run
failed" email. Nothing in this repository is worth stealing, and nothing here
can leak a key because there is none to leak.

What that costs, stated plainly: no Vercel deploy-state check (needs a token),
and no de-duplicated re-notify schedule — GitHub mails on each failed run.

## What it checks

    /api/health                                    200 within 2000ms, body says status ok
    /                                              200 within 2000ms
    /accountants/usa/california/los-angeles        200 within 3000ms

The city page is the canary for on-demand rendering: it exercises the database
in a way the homepage does not.

Every failure is confirmed by a second probe 15 seconds later. One slow
response is not an outage, and an alert that cries wolf gets muted.
