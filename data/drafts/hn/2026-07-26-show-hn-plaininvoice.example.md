---
type: post
pillar: invoice-without-leaving-terminal
goal: awareness
success: Front page for 2+ hours; 20+ substantive comments; 300+ site visits from HN.
status: draft
platform: hn
created: 2026-07-26
published: null
url: null
---

# Show HN: PlainInvoice – invoicing for freelance developers, entirely in the terminal

*Example document — this shows the shape of a draft post (note `status: draft`,
null `published`/`url`, and the goal/success pair). The setup skill deletes
`*.example.md` files after onboarding.*

I freelance as a backend dev and hated switching to a browser app every time I
needed to bill a client. PlainInvoice is a CLI: `pi new`, answer three prompts,
get a PDF and a payment link. Templates are plain TOML in git, so invoices
version like code.

It's free for up to three clients; paid above that. I'd love feedback on the
onboarding flow — first invoice should take under two minutes.

*Convention notes: the filename is `YYYY-MM-DD-<slug>`, the platform directory
(`hn/`) encodes the venue type, and the venue relationship is recorded by
linking this doc under the `## Posts` heading of `data/community-hn.md` — not by
a frontmatter field. The pillar hub in `data/pillars/` inclusion-links this doc,
keeping it reachable in the tree. When published: set `url` and `published`,
flip `status`, and `iwe rename` it into `data/posts/hn/`.*
