---
name: weekly
description: Compose a weekly campaign digest from the workspace — what shipped, engagement numbers, pipeline and outreach state, and ranked next-week priorities. Prints the digest; saves it under data/weekly/ only when the user asks to keep it. Use when the user asks for a "weekly digest", "weekly report", or a start-of-week review.
---

# Weekly digest

A composer, not a fetcher: roll up what the graph already knows. Best run right
after the update skill has refreshed engagement.

## Steps

1. **Window.** Trailing 7 days (or the range the user asks for). The activity
   sources are the frontmatter dates (`published`, `completed`, `created`,
   `reviewed`, `generated.at`) and the dated groups in `data/log.md`. In a git
   repository `git log --since=<date> --name-status -- data` adds what changed;
   without git, the first two are enough.
2. **Shipped.** Posts with `published` in the window; tasks flipped to `done`
   (`completed` in window); plan steps whose stage changed; venues newly
   worked (`planned` → `active`).
3. **Engagement.** Current `engagement.*` for posts published in the last ~14
   days, with notable movers called out, and the week's replies counted by
   `outcome` (and by `names_product`). Posts whose `verdict` was set this week,
   against their `success` line. If numbers look stale, suggest running the
   update skill first.
4. **Pipeline & outreach.** Drafts in flight (`stage: draft`), scheduled
   posts and emails, people with `stage: contacted` and sent emails with
   `outcome: none` awaiting reply (with days-waiting), replies due their
   three-day review (no `outcome`), leads at `new` or `researched` with
   `fit: high`, mentions filed in the window, experiments `running` (and any
   that should have concluded by now or hit their `stop_signal`), and
   competitors with `watch: true` whose `updated` is more than a month old.
5. **Next week.** Ranked shortlist: high-priority `planned` tasks, the active
   plan's unmet Definition-of-done items, and any follow-ups implied by 2–4 (a
   reply to chase, a mention author to contact, an open reading whose
   deciding evidence is now on file). Each item names the fact it rests on.
   Keep it executable — 5 items max.
6. **Print** the digest with those four sections (Shipped, Engagement,
   Pipeline and outreach, Next week). If the user asks to keep
   it, also write `data/weekly/YYYY-MM-DD.md` (`type: weekly-report`,
   `window_start`, `window_end`, `created`, a `description`, and the sections
   `## Shipped`, `## Engagement`, `## Pipeline and outreach`, `## Next week`)
   and inclusion-link it at the top of `data/weekly.md`. A kept report is the
   only write the digest makes, and it gets its line in `data/log.md` like any
   new document (`AGENTS.md`, step 5). The week's state changes were logged
   when they happened; do not log them again.

## Rules

- Numbers come from frontmatter, not memory. If a section is empty, say so in
  one line — an honest "no outreach in flight" beats padding.
- End with the single most leveraged thing to do next week, stated in one
  sentence.
