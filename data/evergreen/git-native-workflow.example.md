---
type: template
description: 'Example evergreen template: the git-native angle, reusable as a post on any platform.'
pillar: invoice-without-leaving-terminal
angle: git-native-workflow
goal: positioning
success: Readers repeat the 'invoices version like code' framing back in comments.
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Your invoices should live in git, like everything else you make

*Example document — this shows the shape of an evergreen template: a reusable,
platform-independent post organized by `angle`, with no `status` (evergreen is
implied by the type). Copy it into a dated draft under `data/drafts/<platform>/`
when using it, then adapt to the venue's rules. The setup skill deletes
`*.example.md` files after onboarding.*

Core argument, reusable across venues: every artifact a developer produces —
code, config, docs — versions in git, except the ones that involve money.
PlainInvoice's templates are TOML files in your repo: diffable, reviewable,
revertable. The post walks one concrete scenario (a client disputes an invoice;
`git log` settles it) and ends with the two-minute quickstart.

Adaptation notes per audience: lead with the dispute story for freelancer
venues; lead with the TOML-in-git mechanics for developer venues.
