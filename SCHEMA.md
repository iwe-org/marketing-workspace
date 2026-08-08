---
type: tracker
stage: living
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

`data/` is a conformant [Open Knowledge
Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle, so every type below also accepts the OKF families described in
[Fields every type carries](#fields-every-type-carries), and the reserved
`data/index.md` and `data/log.md` follow their own shapes. The per-type field
lists name only what is specific to that type.

## Fields every type carries

These are OKF's optional families (SPEC §4.1, §5), declared on every schema and
checked by `.iwe/schemas/okf.yaml`:

- `description` — one sentence. Every document has one; it is what an OKF
  consumer and `data/index.md` show before opening the file.
- `generated` — `{ by, at }`. Write it on **every** edit. `by` is an actor (SPEC
  §7): `claude-code/opus-5` for agent writes, `human:<handle>` for hand-authored
  or human-confirmed content, `process:<id>` for automation.
- `sources` — the materials a document derives from, each
  `{ id, resource, title, author, usage_count, last_modified }` with `resource`
  required. Write it on anything researched from an external page, and attribute
  individual claims with a markdown footnote keyed to an entry's `id` (SPEC
  §5.1).
- `verified` — `{ by, at }` or a list of them, recording who confirmed the
  content. Distinct from `generated`: who wrote it need not be who checked it.
- `resource` — canonical URI of the underlying asset, where one exists.
- `stale_after` — the day the content goes stale. Set on anything that rots:
  competitor snapshots, `data/metrics.md`, venue posting rules.
- `tags`, `usage_window` — available, rarely needed here.

### `stage` versus `status`

`stage` is this workspace's workflow field — the per-type enums below. `status`
is OKF's lifecycle field and takes only `draft | stable | deprecated`; absent
means stable, which is why most documents omit it. Maintain the two together:
whenever you set `stage`, derive `status` from this table and set or clear it.

| type       | `stage` value         | set `status` |
| ---------- | --------------------- | ------------ |
| community  | `cancelled`           | `deprecated` |
| competitor | `archived`            | `deprecated` |
| experiment | `idea`, `planned`     | `draft`      |
| experiment | `abandoned`           | `deprecated` |
| interview  | `planned`             | `draft`      |
| interview  | `cancelled`           | `deprecated` |
| mention    | `removed`             | `deprecated` |
| plan       | `discarded`           | `deprecated` |
| post       | `draft`               | `draft`      |
| post       | `removed`, `archived` | `deprecated` |
| task       | `discarded`           | `deprecated` |

Every other `stage` value means the document is current — omit `status`. A
lapsed `person` is not retired knowledge, so `person` never sets it.

## Reserved files

`data/index.md` and `data/log.md` are OKF reserved filenames and are not concept
documents — they carry no `type` and are checked by `okf-index.yaml` and
`okf-log.yaml` instead.

- `data/index.md` — the bundle-root index. Its only frontmatter is
  `okf_version: "0.2"`. The body is `#` sections of link bullets, each bullet a
  markdown link to a document followed by `-` and that document's `description`.
  Keep it current when adding or retiring a top-level document.
- `data/log.md` — the update history. A title section over `## YYYY-MM-DD`
  groups of bullets, newest first. The `weekly` skill appends to it.

## post — `data/posts/**`, `data/drafts/**`

A publication. `pillar` (your messaging pillar, null until defined), `goal`
(`awareness | conversion | positioning | credibility | retention`), `success`
(one line: the observable signal that means it worked), `stage`
(`draft | scheduled | published | removed | archived`), `platform` (enum incl.
`reddit`, `hn`, `producthunt`, `dev.to`, `linkedin`, `newsletter`, `other`, …),
`created`, `published` (date or null), `resource`, `engagement`
(`upvotes`/`comments`/`views`/`notes`). The venue is a *relationship*: a link
under the community file's `## Posts` heading, not a field. The pillar hub
(`data/pillars/<pillar>.md`) inclusion-links every post carrying it, keeping
posts reachable in the tree.

## template — `data/evergreen/*.md`

A reusable platform-independent post draft. `angle`, `goal`, `success`, optional
`pillar`. No `stage` — evergreen is implied.

## community — `data/community-*.md`, `data/communities/<platform>/*.md`

A venue. `stage` (`planned | active | dormant | blocked | cancelled`),
`category`
(`launch-platform | directory | deals | subreddit | forum | community | social | newsletter | other`),
optional `platform`, `resource`, `size`, `notes` (posting rules live here),
`created`. Body links posts made there under `## Posts` and mentions spotted
there under `## Mentions`.

## person — `data/people/*.md` (except `role-*`)

A CRM entry. `stage`
(`active | planned | contacted | replied | declined | inactive`), optional
`handle`, `email`, `resource`, `platform`, `reach`
(`high | medium | low | unknown`), `notes`, `created`. Roles are inclusion links
from `data/people/role-*.md` hubs, not a field.

## mention — `data/mentions/YYYY-MM-DD-*.md`

An atomic third-party mention. `platform`, direct `resource`, `author` (as it
appears at the venue), `kind`
(`comment | post | thread | listing | review | video | podcast`), `sentiment`
(`positive | neutral | negative | mixed`), verbatim `quote`, `published`
(mention date), `created` (filing date), `stage` (`published | removed`).

## interview — `data/interviews/YYYY-MM-DD-*.md`

A deliberate research event — customer interview or survey (spontaneous quotes
are `mention`s). `stage` (`planned | done | cancelled`), `kind`
(`discovery | new-customer | happy | churned | feature-request | survey`),
`conducted` (date, null while planned). The interviewee is an inline link to
their person doc; verbatim quotes live in the body and get distilled into
`data/product.md`.

## external — `data/external-*.md`

Article-length third-party content. `source`, `resource`, `relation`
(`mentions-us | similar-thesis | adjacent | contrasts`), `takeaway` (one
sentence: why you care), optional `author`, `published`, `pillar`.

## competitor — `data/competitors/*.md`

A competitor profile — the centralized source of truth for one competitor.
`stage` (`active | archived`), `resource`, `updated` (last snapshot date),
`notes` (one-line stance). Body convention: `## Positioning`, `## Pricing`,
`## Strengths`, `## Weaknesses`, `## Changelog` (dated snapshots, newest first).

## experiment — `data/experiments/*.md`

A record of something tried — the permanent answer to "what have we already
tried?". `stage` (`idea | planned | running | concluded | abandoned`),
`hypothesis`, `metric` (primary metric + target), optional `area`, ICE scores
(`impact`/`confidence`/`ease`, 1–10), and on conclusion `verdict`
(`win | loss | inconclusive`), `result`, `concluded`.

## task — `data/backlog/*.md`

A single-sitting executable. `stage`
(`planned | in-progress | done | blocked | discarded`), optional `priority`
(`high | medium | low`), `effort`, `completed`, `notes`.

## plan — `data/plan/*.md`

A campaign-level move. `stage` (`planned | in-progress | done | discarded`),
optional `growth_stage` (`first-10 | first-100 | first-1000`), `estimated`,
`completed`, `effort`, `notes`. Body must contain a `## Definition of done`
section; it may inclusion-link the backlog tasks that fulfil it.

## hub — indexes and role hubs

A landing page that inclusion-links its members (`data/index`,
`data/communities`, `data/people`, `data/mentions`, `data/interviews`,
`data/competitors`, `data/experiments`, `data/people/role-*`, and
`data/pillars/*` — one per messaging pillar, created by setup). `stage: living`,
`created`.

## tracker — living reference docs

`data/product`, `data/strategy`, `data/brand`, `data/offer`, `data/metrics`,
`data/website`, `data/plan`, `data/backlog`, this file. `stage: living`.
