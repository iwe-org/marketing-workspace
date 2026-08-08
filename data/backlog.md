---
type: tracker
description: Atomic, single-sitting tasks, linked here by state.
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Backlog

Atomic, single-sitting tasks (`data/backlog/<slug>.md`), linked here by
priority. The agent picks from the top; status lives in each task's frontmatter
(`planned → in-progress → done`). When a task finishes or is discarded, move its
link to `## Done` so the priority sections stay a true to-do list.

``` bash
iwe find --filter '{type: task, status: planned, priority: high}'
```

## High

[Fill in the product doc](backlog/fill-product-doc.md)

[Set the strategy](backlog/set-strategy.md)

[Prune the venue list to your ICP](backlog/prune-venues.md)

## Medium

[Pick and schedule the first launch venue](backlog/pick-first-launch-venue.md)

[Seed the CRM with people you already know](backlog/seed-crm.md)

[Record your baseline numbers](backlog/record-baselines.md)

[Profile your top competitors](backlog/profile-competitors.md)

## Low

[Set up mention alerts](backlog/set-up-mention-alerts.md)

## Done

[Thank Sarah for the HN mention](backlog/thank-sarah-for-hn-mention.example)
