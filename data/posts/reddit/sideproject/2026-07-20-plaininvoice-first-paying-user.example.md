---
type: post
description: 'Example published post: a first-paying-user retrospective on r/SideProject, with engagement recorded after the fact.'
pillar: invoice-without-leaving-terminal
goal: conversion
success: 10+ signups within a week attributable to the post.
success_evidence: fetchable
verdict: met
verdict_date: 2026-07-27
stage: published
platform: reddit
created: 2026-07-18
published: 2026-07-20
resource: https://www.reddit.com/r/SideProject/comments/example000/
engagement:
  upvotes: 47
  comments: 12
  views: null
  notes: '2026-07-26: steady trickle of signups; two commenters became beta users. 2026-07-27: 14 signups with the r/SideProject referrer in the first week, read in the analytics dashboard.'
generated:
  by: human:author
  at: 2026-07-18T00:00:00Z
---

# I got my first paying user for a CLI invoicing tool — what worked and what didn't

*Example document — this shows the shape of a published post: `url` and
`published` set, engagement tracked in frontmatter (refreshed by the update
skill), and the deeper directory `posts/reddit/<subreddit>/` since Reddit posts
nest per subreddit. The setup skill deletes `*.example.md` files after
onboarding.*

Posted to [r/SideProject](../../../communities/reddit/sideproject.md).

Three weeks ago I posted a rough demo here; today someone paid for PlainInvoice.
The things that actually moved the needle: answering every comment within an
hour, DMing the two people who described the exact pain, and rewriting the
landing page headline with a phrase a commenter used ("invoicing that lives
where I work").

*Verdict: `success_evidence: fetchable` says the evidence lives outside the
workspace (the analytics dashboard); `verdict: met` is the success line checked
literally against it on `verdict_date`. The schema requires both once a verdict
is set.*

*Convention notes: this doc is linked under `## Posts` in
`data/communities/reddit/sideproject.md` (how "all posts made to this venue"
stays queryable) and inclusion-linked from its pillar hub in `data/pillars/`
(how it stays reachable in the tree).*
