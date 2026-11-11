---
type: hub
description: Readings no longer waiting for evidence, kept as the record of what was decided and why.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Readings archive

Readings whose deciding evidence came in, or whose question stopped mattering.
They stay as the record of what was decided and on what facts, so the [readings
hub](readings.md) lists only what is still open. When a reading's `stage` leaves
`open`, move its inclusion link here under the matching section.

``` bash
iwe find --filter '{type: reading, stage: {$ne: open}}' --project '$title,stage,checked'
```

## Supported

[Visitors hesitate because they can't tell whether real freelancers use
PlainInvoice](readings/hesitation-is-about-real-users-example.md)

## Contradicted

## Retired
