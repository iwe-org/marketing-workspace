---
type: reading
description: 'Example open reading: replies that leave the product out get answered more often, but replies that name it start more conversations about trying it.'
question: Does naming PlainInvoice in a reply change how the reply lands?
area: replies
stage: open
impact: medium
impact_estimate: If naming the product is what moves people from replied to will-try, about two more trials a month at the current pace of eight replies.
checked: 2026-08-02
created: 2026-07-31
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Replies that leave the product out get answered more often, but replies that name it start more trials

*Example document — this shows the shape of a reading: the facts kept apart from
what you think they mean, at least one other explanation, and the evidence that
would decide between them. A reading stays on the readings hub while `open`;
when its stage changes, its link moves to the archive. The setup skill deletes
`*-example.md` files after onboarding.*

## Facts

- `iwe find --filter '{type: reply, names_product: false, outcome: {$exists: true}}' --project '$title,outcome'`
  on 2026-08-02: 10 reviewed replies — 5 `replied`, 2 `asked`, 3 `none`.
- The same query with `names_product: true`: 9 reviewed replies — 1 `replied`, 1
  `asked`, 2 `will-try`, 1 `tried`, 4 `none`. One of the four `none` was removed
  by the venue's moderators.
- Three of the four named replies that reached `asked` or better were follow-ups
  to someone who had already answered an unnamed reply.

## Reading

Leaving the product out gets a conversation started; naming it, once someone is
talking to you, is what turns the conversation into a trial.

## Alternatives

- The named replies did better only because they were follow-ups: the people
  were already interested, and naming the product changed nothing.
- The venues differ: most named replies were on venues that allow a product
  name, and those audiences are there to find tools.

## Would decide it

First replies only, on venues that allow a product name, half naming it and half
not, reviewed at three days each.

## Next step

Run it as the [planned
experiment](../experiments/reply-without-link-example.md).
