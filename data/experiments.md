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
iwe find --filter '{type: experiment, status: idea}' --sort impact:-1     # scored backlog
iwe find --filter '{type: experiment, status: running}'                   # in flight
iwe find --filter '{type: experiment, verdict: win}'                      # the playbook
```

## Backlog

## Running

## Concluded

[Landing page social proof above the fold](experiments/landing-social-proof.example)
