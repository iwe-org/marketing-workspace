---
name: weekly
description: Compose a weekly campaign digest from the workspace — what shipped, engagement numbers, pipeline and outreach state, and ranked next-week priorities. Read-only; prints the digest without writing files. Use when the user asks for a "weekly digest", "weekly report", or a start-of-week review.
---

# Weekly digest

A composer, not a fetcher: roll up what the graph already knows. Best run right
after the update skill has refreshed engagement.

## Steps

1. **Window.** Trailing 7 days (or the range the user asks for).
   `git log --since` on the workspace is a reliable activity source alongside
   frontmatter dates.
2. **Shipped.** Posts with `published` in the window; tasks flipped to `done`
   (`completed` in window); plan steps whose stage changed; venues newly
   worked (`planned` → `active`).
3. **Engagement.** Current `engagement.*` for posts published in the last ~14
   days, with notable movers called out. If numbers look stale, suggest
   running the update skill first.
4. **Pipeline & outreach.** Drafts in flight (`stage: draft`), scheduled
   posts, people with `stage: contacted` awaiting reply (with days-waiting),
   mentions filed in the window, experiments `running` (and any that should
   have concluded by now).
5. **Next week.** Ranked shortlist: high-priority `planned` tasks, the active
   plan's unmet Definition-of-done items, and any follow-ups implied by 2–4 (a
   reply to chase, a mention author to contact). Keep it executable — 5 items
   max.
6. **Print** the digest with those five sections.
7. **Log.** Append the week's shipped items to `data/log.md` as bullets under a
   `## YYYY-MM-DD` group for today (create the group if it isn't there, newest
   group first). One line per state change, each linking the document it
   describes — `- **Update**: [Post title](posts/reddit/....md) published.`
   This is the only write the digest makes.

## Rules

- Numbers come from frontmatter, not memory. If a section is empty, say so in
  one line — an honest "no outreach in flight" beats padding.
- End with the single most leveraged thing to do next week, stated in one
  sentence.
