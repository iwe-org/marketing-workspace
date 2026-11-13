---
type: reply
description: 'Example reply record: a helpful answer that does not name the product, which drew a question about the tool.'
stage: published
platform: reddit
resource: https://www.reddit.com/r/webdev/comments/example001/comment/example002/
answers: comment
names_product: false
origin: human
pillar: invoice-without-leaving-terminal
goal: awareness
success: u/hourly_hana asks a follow-up question.
outcome: asked
reviewed: 2026-07-31
published: 2026-07-28
created: 2026-07-28
engagement:
  upvotes: 9
  replies: 2
  thread_upvotes: 58
  thread_comments: 41
  notes: '2026-07-31: u/hourly_hana asked which CLI it was; answered by DM, per the venue''s rules.'
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Answering u/hourly_hana on keeping invoice templates in the repo

*Example document — this shows a reply that answers the question and leaves the
product out, on a venue where naming it outside the weekly showcase gets a
comment removed. `names_product: false` is what lets you compare outcomes with
and without the pitch. The setup skill deletes `*-example.md` files after
onboarding.*

## Context

Person: [u/hourly_hana](../leads/hourly-hana-example.md)

In [How do you invoice clients without paying for another
SaaS?](../threads/reddit-webdev-invoicing-without-saas-example.md) on
[r/webdev](../communities/reddit/webdev.md). u/hourly_hana asked whether anyone
keeps invoices "next to the code, like a changelog".

The agent drafted three talking points (templates as plain text, one file per
client, the git history settling disputes); the founder wrote the words, per the
[reply decision](../notes/founder-writes-replies-example.md).

## Our reply

> I do exactly that. One plain-text template per client in the repo, and the
> invoice is generated from it. When a client once questioned an old rate,
> `git log` on the template answered it in ten seconds.

## Discussion

- 2026-07-29 — u/hourly_hana: "which tool generates them?" Answered by DM.
- 2026-07-30 — another commenter asked for the folder layout; answered in-thread
  without naming the product.
