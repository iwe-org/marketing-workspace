---
type: hub
description: Every thread where you met people — the conversations your replies sit in.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Threads

One page per thread, post or comment chain where you met someone
(`data/threads/<platform>-<slug>.md`). A thread page links its venue and
inclusion-links the [replies](replies.md) you posted in it; a [lead](leads.md)'s
"Where we found them" links the thread they were spotted in. File the thread
page first, then the lead, then the reply.

``` bash
iwe find --filter '{type: thread, platform: reddit}' --project '$title,opened'
```

[Ask HN — what do you use for
invoicing?](threads/hn-ask-hn-invoicing-example.md)

[How do you invoice clients without paying for another
SaaS?](threads/reddit-webdev-invoicing-without-saas-example.md)

[What tools do you actually pay for as a solo
developer?](threads/reddit-sideproject-tools-you-pay-for-example.md)

[Ask HN: What's your freelance back-office
stack?](threads/hn-ask-hn-freelance-back-office-example.md)

[Client disputes an invoice from March, how do you keep
records?](threads/reddit-webdev-disputed-invoice-example.md)

[How did you handle invoicing for your first
clients?](threads/ih-invoicing-first-clients-example.md)

[Self-hosted invoicing that isn't a whole
ERP?](threads/reddit-selfhosted-light-invoicing-example.md)

[Week 12 of building a CLI tool: what I'd
change](threads/reddit-buildinpublic-cli-week-twelve-example.md)

[Show HN: A plain-text ledger for
freelancers](threads/hn-show-hn-plain-text-ledger-example.md)

[When did you add a paid
plan?](threads/reddit-indiehackers-when-to-charge-example.md)

[Do you keep non-code stuff in
git?](threads/reddit-webdev-non-code-in-git-example.md)

[Would you use a CLI-only
product?](threads/reddit-sideproject-cli-only-product-example.md)

[Pricing a tool for developers who hate
subscriptions](threads/ih-pricing-for-developers-example.md)
