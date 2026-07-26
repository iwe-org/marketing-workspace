---
name: update
description: Refresh engagement metrics (upvotes, comments, views) on published posts by fetching their canonical URLs, then print an engagement report. Use when the user asks to "update engagement", "refresh metrics", or "how are the posts doing". Default scope is posts published in the last 14 days.
---

# Update engagement

Keep `engagement.*` frontmatter close to reality so the weekly digest and the
user's judgment work from current numbers.

## Steps

1. **Scope.**
   `iwe find --filter '{type: post, status: published}' --project '$title,platform,url,published' -f json`,
   keep posts with a non-null `url` published in the last 14 days (or the range
   the user asked for).
2. **Fetch.** For each post, fetch its `url` and extract what the platform
   shows: upvotes/points, comment count, views/impressions where visible.
   Platforms differ — read what's actually on the page; if a fetch fails or the
   page hides numbers, record the failure in `engagement.notes` instead of
   guessing. Reddit blocks most server-side fetches: retry the post through
   `https://embed.reddit.com/<post-path>` — the embed route serves score and
   comment count when `reddit.com` refuses. If a post is gone at the source
   (deleted/removed by moderators), set `status: removed` and say so in the
   report.
3. **Write back.** Update each post's `engagement.upvotes`,
   `engagement.comments`, `engagement.views` (only fields you actually
   observed), and a dated one-line `engagement.notes` when something needs
   context.
4. **Validate & commit.** `iwe schema validate`; commit
   `update: engagement refresh — <n> posts`.
5. **Report.** Print a table — post title, venue, upvotes, comments, and the
   change since the last recorded values — plus one line calling out anything
   notable (a post taking off, a venue consistently flat).

## Rules

- Never invent numbers. An unfetchable page is a note, not an estimate.
- Don't refresh more often than the venue meaningfully changes — daily at most.
