---
name: backfill
description: Backfill the workspace from the internet — read the product website to fill gaps in the product doc, sweep HN/Reddit/the web for existing mentions, file already-published posts with their engagement, and stub competitor profiles from public pages. Use when the user says "backfill the workspace", "import what already exists", or right after setup for a product with any public history.
---

# Backfill from the internet

The user's marketing didn't start today. This skill imports the campaign
history that already exists — mentions, posts, competitor facts, baselines —
so the graph starts true, not empty. Requires web access. Collaboration over
automation: propose before filing in bulk, and never state what a page didn't
show.

## Steps

1. **Scope with the user.** Confirm: the product name and searchable aliases
   (old names, handles, the domain), the website URL, competitor names, and
   which platforms they've already posted on (handles or profile links).
   Ideally run after setup; if `data/product.md` is still unfilled, the
   website pass may *propose* drafts for its gap sections, clearly marked for
   the user to confirm.
2. **Website pass.** Fetch the site. Fill *gaps only*: `data/product.md`
   sections still showing ✏️, pricing into `data/offer.md`, the page
   inventory into `data/website.md` (Key pages), org facts where visible.
   Where the site contradicts something the founder wrote, ask — never
   silently overwrite.
3. **Mention sweep.** Search the product name + aliases: HN (Algolia search),
   Reddit, general web and blogs. Open every hit before filing (Reddit blocks
   most server-side fetches — a thread that refuses to load usually works via
   `https://embed.reddit.com/<post-path>`). Atomic
   comment/thread → `data/mentions/YYYY-MM-DD-<slug>.md` (verbatim `quote`,
   direct `resource`, `author` exactly as displayed, honest `sentiment`);
   article-length piece → `data/external-<slug>.md` with a one-line
   `takeaway`. Link mentions from `data/mentions.md` and the venue's
   `## Mentions` heading; file authors worth following up in `data/people/`
   (roles via the role hubs) — a positive mention is warmed-up outreach.
4. **Published-post import.** From the user's links and their profile pages,
   file each already-published post as
   `data/posts/<platform>/YYYY-MM-DD-<slug>.md` (`stage: published`, the real
   `published` date and `resource`; body = the actual post text, or a faithful
   summary when it's long). Record engagement numbers as displayed. The schema
   requires `goal` and `success` — set them retroactively with the user ("what
   was this post for?"). Link each post from its venue's `## Posts` heading
   and its pillar hub, and flip the venue's `stage` to `active`.
5. **Competitor snapshots.** For each competitor named, fetch their public
   pages and fill `data/competitors/<slug>.md`: Positioning, Pricing,
   Strengths and Weaknesses honestly, a dated Changelog entry,
   `updated: <today>`.
6. **Baselines.** Public numbers found along the way (stars, followers,
   subscribers, review counts) → `data/metrics.md`, each with a dated
   snapshot line.
7. **Report, validate, commit.** Summarize what was filed (n mentions, n
   posts, n people, n competitor profiles, metrics captured) **and what could
   not be searched** (auth-walled platforms, rate limits, dead links) so the
   user can fill those by hand. `iwe schema validate` must pass; commit
   `backfill: <n> mentions, <n> posts, <n> competitors`.

## Rules

- **Never invent.** Quotes verbatim from the fetched page; numbers only as
  displayed; dates from the source. A find without a URL you actually opened
  does not get filed.
- Fill gaps, don't overwrite — the founder's language wins; conflicts become
  questions, not silent edits.
- Propose the find-list before bulk-filing (what, where found, how it would be
  filed); file after the user confirms. A single obvious find can be filed
  directly.
- A mention the user remembers but that's gone at the source: file it only
  with a working archive link, else skip it and say so.
- Degrade honestly: report the platforms you couldn't search rather than
  implying full coverage.
