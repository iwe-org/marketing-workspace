---
type: experiment
description: 'Example experiment: discount the annual plan.'
stage: concluded
area: pricing
hypothesis: Showing an annual price with two months free on the pricing page will raise paid-tier waitlist joins, because freelancers like a known yearly cost.
metric: Share of pricing-page visitors who join the paid-tier waitlist; baseline 10%, target 13%
stop_signal: A join rate below 8% after 30 pricing-page visitors ends it as a loss.
impact: 5
confidence: 5
ease: 8
verdict: loss
result: 4 of 41 pricing-page visitors joined the waitlist, against 4 of 38 before; the join rate did not move.
started: 2026-06-01
concluded: 2026-06-29
created: 2026-05-28
generated:
  by: human:author
  at: 2026-08-02T00:00:00Z
---

# Discount the annual plan

*Example document — one of the dummy records that fill the charts on the
experiments hub. The setup skill deletes `*-example.md` files after onboarding.*

Run before the paid tier opened on 2026-07-20, against its waitlist. A loss with
a clear learning: the yearly cost was not the hesitation.
