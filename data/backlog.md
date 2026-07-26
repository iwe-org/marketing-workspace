---
type: tracker
status: living
created: 2026-07-26
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

[Fill in the product doc](backlog/fill-product-doc)

[Set the strategy](backlog/set-strategy)

[Prune the venue list to your ICP](backlog/prune-venues)

## Medium

[Pick and schedule the first launch venue](backlog/pick-first-launch-venue)

[Seed the CRM with people you already know](backlog/seed-crm)

[Record your baseline numbers](backlog/record-baselines)

[Profile your top competitors](backlog/profile-competitors)

## Low

[Set up mention alerts](backlog/set-up-mention-alerts)

## Done

[Thank Sarah for the HN mention](backlog/thank-sarah-for-hn-mention.example)
