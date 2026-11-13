---
type: hub
description: Everything tried and its verdict — the permanent answer to "what have you already tested".
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Experiments

Everything tried and its verdict — the permanent answer to "what have you
already tried?". One record per experiment or reversible decision
(`data/experiments/<slug>.md`): hypothesis and primary metric in frontmatter,
context and learning in the body. Score ideas with `impact`/`confidence`/`ease`
(1–10) to prioritize the backlog; when concluded, set `verdict`
(win/loss/inconclusive) and `result` — a loss with a clear learning is a
successful experiment.

``` bash
iwe find --filter '{type: experiment, stage: idea}' --sort impact:-1     # scored backlog
iwe find --filter '{type: experiment, stage: running}'                   # in flight
iwe find --filter '{type: experiment, verdict: win}'                      # the playbook
```

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/experiments/}}
    project: {stage: 1, verdict: 1}
transform:
  - calculate: "datum.verdict != null ? datum.verdict : 'no verdict'"
    as: result
  - calculate: "indexof(['win', 'loss', 'inconclusive', 'no verdict'], datum.result)"
    as: rank
title: Experiments by stage
height: 130
mark: bar
encoding:
  y: {field: stage, type: nominal, title: null, sort: [idea, planned, running, concluded, abandoned]}
  x: {aggregate: count, type: quantitative, title: experiments, axis: {tickMinStep: 1}}
  color:
    field: result
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [win, loss, inconclusive, no verdict]
      range: ["#2e9d5b", "#d9534f", "#e6c34a", "#a6a6a6"]
  order: {field: rank, aggregate: min, sort: ascending}
  tooltip: [{field: stage}, {field: result, title: verdict}, {aggregate: count, title: experiments}]
```

## Backlog

[Say "solo only" on the pricing
page](experiments/pricing-page-solo-only-example.md)

[Name the product in first replies, or
don't](experiments/reply-without-link-example.md)

## Running

[Send a day-two onboarding
email](experiments/onboarding-email-day-two-example.md)

## Abandoned

[Put a demo video above the fold](experiments/landing-demo-video-example.md)

## Concluded

[Discount the annual plan](experiments/annual-plan-discount-example.md)

[Landing page social proof above the
fold](experiments/landing-social-proof-example.md)
