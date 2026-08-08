---
type: hub
description: One profile per competitor, refreshed through dated snapshots.
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Competitors

One profile per competitor (`data/competitors/<slug>.md`) — the centralized
source of truth their comparison pages, pricing research, and SEO analysis all
read, so an update propagates everywhere. Keep profiles honest: what they're
genuinely better at matters more than a feature-checklist win.

Profile body convention: `## Positioning` · `## Pricing` · `## Strengths` ·
`## Weaknesses` · `## Changelog` (dated snapshot notes, newest first — pricing
changes are the most volatile and the most valuable to catch). Set the `updated`
field on each refresh.

``` bash
iwe find --filter '{type: competitor, status: active}' --project '$title,url,updated' -f json
```

[BillingBear](competitors/billingbear.example)
