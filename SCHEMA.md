---
type: tracker
status: living
created: 2026-07-26
---

# Frontmatter schema reference

Every document carries YAML frontmatter; the `type` field discriminates the
schema. The shapes below are machine-enforced by `iwe schema validate` (schemas
in `.iwe/schemas/`, bound by path globs in `.iwe/config.toml`). This file is the
human summary.

Titles come from the H1 header; a `title` frontmatter field is kept only where
the H1 doesn't carry it. Dates are ISO (`YYYY-MM-DD`). Most data directories
ship `*.example.md` docs demonstrating their shape with one fictional product
(PlainInvoice) — the examples cross-link each other so the graph conventions are
shown live, validate against the same schemas, and the setup skill deletes them
after onboarding.

## post — `data/posts/**`, `data/drafts/**`

A publication. `pillar` (your messaging pillar, null until defined), `goal`
(`awareness | conversion | positioning | credibility | retention`), `success`
(one line: the observable signal that means it worked), `status`
(`draft | scheduled | published | removed | archived`), `platform` (enum incl.
`reddit`, `hn`, `producthunt`, `dev.to`, `linkedin`, `newsletter`, `other`, …),
`created`, `published` (date or null), `url`, `engagement`
(`upvotes`/`comments`/`views`/`notes`). The venue is a *relationship*: a link
under the community file's `## Posts` heading, not a field. The pillar hub
(`data/pillars/<pillar>.md`) inclusion-links every post carrying it, keeping
posts reachable in the tree.

## template — `data/evergreen/*.md`

A reusable platform-independent post draft. `angle`, `goal`, `success`, optional
`pillar`. No `status` — evergreen is implied.

## community — `data/community-*.md`, `data/communities/<platform>/*.md`

A venue. `status` (`planned | active | dormant | blocked | cancelled`),
`category`
(`launch-platform | directory | deals | subreddit | forum | community | social | newsletter | other`),
optional `platform`, `url`, `size`, `notes` (posting rules live here),
`created`. Body links posts made there under `## Posts` and mentions spotted
there under `## Mentions`.

## person — `data/people/*.md` (except `role-*`)

A CRM entry. `status`
(`active | planned | contacted | replied | declined | inactive`), optional
`handle`, `email`, `url`, `platform`, `reach` (`high | medium | low | unknown`),
`notes`, `created`. Roles are inclusion links from `data/people/role-*.md` hubs,
not a field.

## mention — `data/mentions/YYYY-MM-DD-*.md`

An atomic third-party mention. `platform`, direct `url`, `author` (as it appears
at the venue), `kind`
(`comment | post | thread | listing | review | video | podcast`), `sentiment`
(`positive | neutral | negative | mixed`), verbatim `quote`, `published`
(mention date), `created` (filing date), `status` (`published | removed`).

## interview — `data/interviews/YYYY-MM-DD-*.md`

A deliberate research event — customer interview or survey (spontaneous quotes
are `mention`s). `status` (`planned | done | cancelled`), `kind`
(`discovery | new-customer | happy | churned | feature-request | survey`),
`conducted` (date, null while planned). The interviewee is an inline link to
their person doc; verbatim quotes live in the body and get distilled into
`data/product.md`.

## external — `data/external-*.md`

Article-length third-party content. `source`, `url`, `relation`
(`mentions-us | similar-thesis | adjacent | contrasts`), `takeaway` (one
sentence: why you care), optional `author`, `published`, `pillar`.

## competitor — `data/competitors/*.md`

A competitor profile — the centralized source of truth for one competitor.
`status` (`active | archived`), `url`, `updated` (last snapshot date), `notes`
(one-line stance). Body convention: `## Positioning`, `## Pricing`,
`## Strengths`, `## Weaknesses`, `## Changelog` (dated snapshots, newest first).

## experiment — `data/experiments/*.md`

A record of something tried — the permanent answer to "what have we already
tried?". `status` (`idea | planned | running | concluded | abandoned`),
`hypothesis`, `metric` (primary metric + target), optional `area`, ICE scores
(`impact`/`confidence`/`ease`, 1–10), and on conclusion `verdict`
(`win | loss | inconclusive`), `result`, `concluded`.

## task — `data/backlog/*.md`

A single-sitting executable. `status`
(`planned | in-progress | done | blocked | discarded`), optional `priority`
(`high | medium | low`), `effort`, `completed`, `notes`.

## plan — `data/plan/*.md`

A campaign-level move. `status` (`planned | in-progress | done | discarded`),
optional `stage` (`first-10 | first-100 | first-1000`), `estimated`,
`completed`, `effort`, `notes`. Body must contain a `## Definition of done`
section; it may inclusion-link the backlog tasks that fulfil it.

## hub — indexes and role hubs

A landing page that inclusion-links its members (`data/index`,
`data/communities`, `data/people`, `data/mentions`, `data/interviews`,
`data/competitors`, `data/experiments`, `data/people/role-*`, and
`data/pillars/*` — one per messaging pillar, created by setup).
`status: living`, `created`.

## tracker — living reference docs

`data/product`, `data/strategy`, `data/brand`, `data/offer`, `data/metrics`,
`data/website`, `data/plan`, `data/backlog`, this file. `status: living`.
