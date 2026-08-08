---
type: mention
description: 'Example mention record: a customer recommending the product unprompted in a Hacker News invoicing-tools thread.'
stage: published
platform: hn
resource: https://news.ycombinator.com/item?id=00000000
author: sarahk_dev
kind: comment
sentiment: positive
quote: PlainInvoice is the only one that doesn't make me leave the terminal — templates are TOML in my repo, so invoices version like code.
published: 2026-07-23
created: 2026-07-23
sources:
- id: hn-thread
  resource: https://news.ycombinator.com/item?id=00000000
  title: Ask HN — what do you use for invoicing?
  author: human:sarah-k
  last_modified: 2026-07-23
generated:
  by: human:author
  at: 2026-07-23T00:00:00Z
---

# Sarah recommends PlainInvoice in an HN invoicing-tools thread

*Example document — this shows the shape of a mention: an atomic, dated record
of a spontaneous third-party mention, with the direct `url` and the verbatim
`quote` (deliberate research you conducted is `type: interview` instead). The
setup skill deletes `*.example.md` files after onboarding.*

An "Ask HN: invoicing for freelancers?" thread;
[Sarah K.](../people/sarah-k.example) recommended us unprompted, third comment
from the top. Note she repeated the "invoices version like code" framing back —
the exact success signal the git-native-workflow evergreen template names.

Follow-up: thanked her in-thread and by email the same day
([task](../backlog/thank-sarah-for-hn-mention.example)).

*Convention notes: filename is `YYYY-MM-DD-<author>-<venue>-<gist>`. Three graph
edges accompany a mention, all live for this example: an inclusion link from
`data/mentions.md` (Atomic section), a link under `## Mentions` in the venue
file (`data/community-hn.md`), and the inline author link to their CRM doc as
above — a positive mention is outreach warmed up for you, so the author always
gets filed.*
