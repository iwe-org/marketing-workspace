---
name: update
description: Refresh engagement metrics (upvotes, comments, views) on published posts and thread replies by fetching their canonical URLs, propose reply outcomes that are due, then print an engagement report. Use when the user asks to "update engagement", "refresh metrics", or "how are the posts doing". Default scope is the last 14 days.
---

# Update engagement

Keep `engagement.*` frontmatter close to reality so the weekly digest and the
user's judgment work from current numbers. Replies are refreshed the same way,
but their measure is `outcome`, not score.

## Steps

1. **Scope.**
   `iwe find --filter '{type: post, stage: published}' --project '$title,platform,resource,published' -f json`,
   keep posts that carry a `resource`, published in the last 14 days (or the range
   the user asked for).
2. **Fetch.** For each post, fetch its `resource` and extract what the platform
   shows: upvotes/points, comment count, views/impressions where visible.
   Platforms differ — read what's actually on the page; if a fetch fails or the
   page hides numbers, record the failure in `engagement.notes` instead of
   guessing. Reddit blocks most server-side fetches: retry the post through
   `https://embed.reddit.com/<post-path>` — the embed route serves score and
   comment count when `reddit.com` refuses. If a post is gone at the source
   (deleted/removed by moderators), set `stage: removed` and
   `status: deprecated`, and say so in the report.
3. **Write back.** Update each post's `engagement.upvotes`,
   `engagement.comments`, `engagement.views` (only fields you actually
   observed), and a dated one-line `engagement.notes` when something needs
   context.
4. **Replies.** `iwe find --filter '{type: reply, stage: published}' --project '$title,resource,published,outcome' -f json`,
   last 14 days. Fetch each `resource` and update `engagement.upvotes`,
   `engagement.replies`, and the thread's `thread_upvotes` and
   `thread_comments`. Append what the comment drew to `## Discussion` as dated
   bullets. For a reply published three or more days ago with no `outcome`,
   propose one from what the discussion literally shows (`none`, `replied`,
   `asked`, `will-try`, `tried`) and set it with `reviewed: <today>` once the
   user agrees; raise an existing outcome when the conversation has moved. A
   comment gone at the source gets `stage: removed` and `status: deprecated`.
5. **Success lines.** For posts published more than a week ago with no
   `verdict`, check the `success` line against the evidence: set
   `success_evidence`, and a `verdict` with `verdict_date` when the evidence
   could be read. A line that cannot be observed gets `unobservable` and no
   verdict.
6. **Validate.** `iwe schema validate`. In a git repository, commit
   `update: engagement refresh — <n> posts, <n> replies`.
7. **Report.** Print a table — post or reply title, venue, upvotes, comments,
   and the change since the last recorded values — then the outcomes set this
   run, plus one line calling out anything notable (a post taking off, a venue
   consistently flat).

## Rules

- Never invent numbers. An unfetchable page is a note, not an estimate.
- An outcome is what someone wrote, not what the score suggests. `tried`
  needs a person saying they used the product.
- Don't refresh more often than the venue meaningfully changes — daily at most.
