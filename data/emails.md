---
type: hub
description: Outreach email sent and received, one record each, with goal, success and outcome.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Emails

Outreach email, one record per message (`data/emails/YYYY-MM-DD-<slug>.md`):
pitches, follow-ups, and replies worth keeping. The recipient is an inline link
to their person or lead record; the person's `stage` still tracks the
relationship. Outbound email states `goal` and `success` like any publication.

``` bash
iwe find --filter '{type: email, stage: scheduled}' --project '$title,target'
iwe find --filter '{type: email, stage: sent, outcome: none}'                  # waiting for an answer
```

[A terminal invoicing tool for your terminal
corner](emails/2026-07-25-devon-pitch.example)

[Follow-up to Devon](emails/2026-08-04-devon-follow-up.example)
