---
type: hub
description: The concrete reasons to buy, each with its proof and the copy it feeds, grouped by kind.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Selling points

One document per reason to buy (`data/selling-points/<slug>.md`), so every post
starts from an argument that already exists instead of inventing one. `evidence`
says what backs the claim, and `## Proof` names what is still missing; a point
resting on a `hypothesis` stays a `candidate` and stays out of copy.

``` bash
iwe find --filter '{type: selling-point, stage: {$in: [validated, promoted]}}' --project '$title,kind,strength'
```

## Differentiators

[Invoices version like code](selling-points/invoices-version-like-code.example)

## Outcomes

[The first invoice takes under two
minutes](selling-points/first-invoice-in-two-minutes.example)

## Workflows

## Capabilities
