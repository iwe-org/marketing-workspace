---
type: hub
description: Short durable notes — decisions, learnings and gotchas a future session should know.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Notes

Short durable notes (`data/notes/<slug>.md`): a decision the user made, a
learning no other document records, or a gotcha that cost time. A note is never
deleted; when something replaces it, set `stage: superseded` and link the
replacement.

``` bash
iwe find --filter '{type: note, stage: active}' --project '$title,kind'
```

## Decisions

[The founder writes the final words of every
reply](notes/founder-writes-replies-example.md)

## Learnings

[Commenters' phrases make better headlines than
ours](notes/commenter-phrases-make-headlines-example.md)

## Gotchas
