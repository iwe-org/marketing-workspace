---
type: hub
status: living
created: 2026-07-26
---

# Interviews

Deliberate research events — customer interviews and surveys — one dated record
each (`data/interviews/YYYY-MM-DD-<slug>.md`). Spontaneous third-party quotes
are [Mentions](mentions); this is where you file what you *went and asked*. Link
the interviewee's person doc inline in the body; capture verbatim quotes
generously, then distill the best language into `data/product.md` (Customer
language, Objections, Switching forces) — headlines get written from what people
said here.

The `kind` field names the research motion: `discovery` (pre-product problem
interviews), `new-customer` (why they switched — JTBD), `happy` (what to double
down on), `churned` (why they left), `feature-request` (the need behind the
ask), `survey`.

``` bash
iwe find --filter '{type: interview, kind: churned}'      # every churn conversation
iwe find --filter '{type: interview, status: planned}'    # scheduled research
```

[Marco (agency owner) — churned after two months](interviews/2026-07-24-marco-churned.example)

[Sarah K. (freelance backend dev) — why she switched from BillingBear](interviews/2026-07-21-sarah-new-customer.example)
