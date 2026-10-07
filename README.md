# Exercise: Email Notification Service (60 minutes)

## Context

You're an engineer at a small SaaS company. Users can request a data export
from the dashboard. When the export finishes, we email them a download link.

Right now, `app/exports.py` calls the email provider directly from the request
handler. This is causing two problems in production:

1. **Slow responses.** When the email provider is slow, the "export complete"
   request hangs until the email is sent.
2. **Lost emails.** When the email provider has an outage, the retry loop
   gives up and the email is never sent. Last month a 40-minute provider
   outage meant ~300 users never got their download link, and nobody knew
   until support tickets arrived.

Run `python scripts/demo.py` to see both problems reproduced locally.

## Your task

Design and build a minimal internal notification service that other parts of
the app can call to send an email. Replace the direct provider call in
`exports.py` with a call to your service.

You'll work with a coding agent. Use it however you'd use it on the job — we
are interested in how you direct it and how you check its work, not whether
you can type faster than it.

## Requirements

1. Sending an email must not block the caller.
2. If the email provider is down — for seconds, or for an hour — no accepted
   notification is lost. This must hold even if our own process restarts
   during the outage.
3. Keep it runnable locally with no external services. The fake provider in
   `app/email_provider.py` and in-process or file-based storage are fine.

## Deliverables, in priority order

1. Working code for the send path, with at least one test that proves
   requirement 2.
2. A short written list of the decisions you made, in `DECISIONS.md`. Five
   bullets max. One bullet must describe something the agent proposed that
   you changed or rejected, and why.
3. Anything else you had time for.

## Out of scope

Deduplication of retried requests, templates, HTML rendering, unsubscribe
handling, other channels (SMS, push), authentication between services.

## Setup

```
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
pytest
python scripts/demo.py
```

Python 3.11+. Standard library only, plus pytest. You may add dependencies if
you can explain why.

## How we evaluate

- Whether the code does what the requirements ask — especially "no accepted
  notification is lost."
- Clarity of your decisions and the trade-offs you name.
- How you specified work for the agent, and what you verified yourself.
- How you handled ambiguity. Ask us questions; that's expected.
