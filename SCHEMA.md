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

Types you browse as a table also have a UI sidecar,
`.iwe/schemas/<name>.ui.yaml` — the columns an inclusion row shows, the display
titles and colors of enum values, and the fields hidden from the properties
band. The validator never reads it; it only changes how the iWe app displays the
type.

Hubs draw live charts from these fields: ```` ```chart ```` blocks whose data is
a query over the documents (`$key: {$startsWith: data/replies/}`, a `type`
filter, or an edge such as `$referencedBy`) or a data file such as
`data/metrics-weekly.csv`. A field a chart reads is listed on its hub; renaming
one means updating the chart.

Some rules depend on another field's value. They are written as `if`/`then` in
the schema and listed under each type below as **Conditional** — for example a
`done` task must carry `completed`.

Titles come from the H1 header; a `title` frontmatter field is kept only where
the H1 doesn't carry it. Dates are ISO (`YYYY-MM-DD`). Most data directories
ship `*-example.md` docs demonstrating their shape with one fictional product
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

| type          | `stage` value             | set `status` |
| ------------- | ------------------------- | ------------ |
| community     | `cancelled`               | `deprecated` |
| competitor    | `archived`                | `deprecated` |
| email         | `draft`, `scheduled`      | `draft`      |
| email         | `archived`                | `deprecated` |
| experiment    | `idea`, `planned`         | `draft`      |
| experiment    | `abandoned`               | `deprecated` |
| interview     | `planned`                 | `draft`      |
| interview     | `cancelled`               | `deprecated` |
| mention       | `removed`                 | `deprecated` |
| note          | `superseded`              | `deprecated` |
| plan          | `discarded`               | `deprecated` |
| post          | `draft`                   | `draft`      |
| post          | `removed`, `archived`     | `deprecated` |
| reading       | `contradicted`, `retired` | `deprecated` |
| reference     | `dropped`                 | `deprecated` |
| reply         | `removed`, `deleted`      | `deprecated` |
| selling-point | `candidate`               | `draft`      |
| selling-point | `retired`                 | `deprecated` |
| task          | `discarded`               | `deprecated` |

Every other `stage` value means the document is current — omit `status`. A
lapsed `person`, a dropped `lead` and an inactive `audience` are not retired
knowledge, so those three never set it. `thread` and `weekly-report` have no
`stage`.

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
(`upvotes`/`comments`/`views`/`shares`/`notes`). Once the success line can be
checked: `success_evidence` (`on-file | fetchable | unobservable` — where the
evidence lives), `verdict` (`met | partly-met | not-met | undecidable`, the line
checked literally) and `verdict_date`. **Conditional**: `published` needs a
`published` date and a `resource`; a `verdict` needs `success_evidence` and
`verdict_date`. The venue is a *relationship*: a link under the community file's
`## Posts` heading, not a field. The pillar hub (`data/pillars/<pillar>.md`)
inclusion-links every post carrying it, keeping posts reachable in the tree.

## template — `data/evergreen/*.md`

A reusable platform-independent post draft. `angle`, `goal`, `success`, optional
`pillar`. No `stage` — evergreen is implied.

## community — `data/community-*.md`, `data/communities/<platform>/*.md`

A venue. `stage` (`planned | active | dormant | blocked | cancelled`),
`category`
(`launch-platform | directory | deals | subreddit | forum | community | social | newsletter | other`),
optional `platform`, `resource`, `size`, `notes` (posting rules live here),
`members` (the audience size as a number) with `checked` (the date it was read),
`pillar`, `created`. Body links posts made there under `## Posts` and mentions
spotted there under `## Mentions`.

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
(`positive | neutral | negative | mixed`), optional `pillar`, verbatim `quote`,
`published` (mention date), `created` (filing date), `stage`
(`published | removed`).

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
`stage` (`active | dormant | archived`), `resource`, `updated` (last snapshot
date), `notes` (one-line stance), optional `relation`
(`direct | adjacent | incumbent | substitute`), `threat` (`high | medium | low`
— set it only with the reason written in the body) and `watch` (`true` puts it
in the weekly digest). The body must have `## Positioning`, `## Pricing`,
`## Strengths`, `## Weaknesses` and `## Changelog` (dated snapshots, newest
first); other sections may follow.

## experiment — `data/experiments/*.md`

A record of something tried — the permanent answer to "what have we already
tried?". `stage` (`idea | planned | running | concluded | abandoned`),
`hypothesis`, `metric` (primary metric + target), optional `area`, `stop_signal`
(written before it starts: the observation that ends it early or calls it a
loss), ICE scores (`impact`/`confidence`/`ease`, 1–10), and on conclusion
`verdict` (`win | loss | inconclusive`), `result`, `concluded`. **Conditional**:
`concluded` needs `verdict`, `result` and `concluded`; `running` needs
`started`.

## task — `data/backlog/*.md`

A single-sitting executable. `stage`
(`planned | in-progress | done | blocked | discarded`), optional `priority`
(`high | medium | low`), `effort`, `completed`, `notes`. **Conditional**: `done`
needs `completed`.

## plan — `data/plan/*.md`

A campaign-level move. `stage` (`planned | in-progress | done | discarded`),
optional `growth_stage` (`first-10 | first-100 | first-1000`), `estimated`,
`completed`, `effort`, `notes`. Body must contain a `## Definition of done`
section and may close with a `## Retro`; it may inclusion-link the backlog tasks
that fulfil it. **Conditional**: `done` needs `completed`.

## thread — `data/threads/<platform>-<slug>.md`

One thread, post or comment chain where you met people. `platform`, `resource`
(permalink), optional `opened` (when the thread started), `created`. The body
links the venue and may close with `## Replies`, inclusion-linking the reply
records posted in it, oldest first.

## reply — `data/replies/YYYY-MM-DD-<platform>-<slug>.md`

One comment you posted in someone else's thread. `stage`
(`published | removed | deleted`), `platform`, `resource` (permalink to your
comment), `answers` (`post | comment`), `names_product` (boolean — did it name
or link the product), `origin` (`human | agent | agent-edited` — who wrote the
words that went up), `goal`, optional `success` and `pillar`, `published`,
`created`, `engagement` (`upvotes`, `replies`, `thread_upvotes`,
`thread_comments`, `notes`). `outcome`
(`none | replied | asked | will-try | tried`) is the measure, set at a three-day
review with the date in `reviewed`. **Conditional**: `outcome` needs `reviewed`.
Body: `## Context` (opens with a `Person:` link), `## Our reply` (verbatim, as a
quote), optional `## Discussion`. The thread is not a field — the thread page
links the reply.

## lead — `data/leads/<handle-slug>.md`

A person spotted in a thread who may want the product. `stage`
(`new | researched | active | converted | dropped`), `platform`, `handle`,
`resource` (profile), `fit` (`high | medium | low`), optional `pillar`, `notes`,
`created`. **Conditional**: `dropped` needs `notes`. Body:
`## Where we found them` (links the thread page) and `## What they need`, then
any notes. Whether they answered is read from their replies' `outcome`, never
stored here. A converted lead gets a `data/people/` record.

## email — `data/emails/YYYY-MM-DD-<slug>.md`

One outreach email. `stage` (`draft | scheduled | sent | archived`), `direction`
(`outbound | inbound`), `target` (the other party's name), optional `subject`,
`pillar`, `goal`, `success`, `outcome` (`none | replied | declined | accepted`),
`sent`, `created`. **Conditional**: `outbound` needs `goal` and `success`;
`sent` needs the `sent` date.

## audience — `data/audiences/<slug>.md`

One audience segment. `stage` (`active | planned | inactive`), optional
`pillar`, `fit`, `notes`, `created`. **Conditional**: `inactive` needs `notes`.
Body: `## Who they are`, `## What resonates`, `## What to avoid`,
`## Evergreen pitches`, `## Communities`.

## selling-point — `data/selling-points/<slug>.md`

One concrete reason to buy. `kind`
(`workflow | capability | differentiator | outcome`), `stage`
(`candidate | validated | promoted | retired`), `strength`
(`high | medium | low`), `evidence`
(`shipped | used | customer-reported | hypothesis`), optional `pillar`,
`created`. **Conditional**: `hypothesis` keeps `stage` at `candidate` or
`retired`. Body: `## The claim`, `## How it works`, `## Why it matters`,
`## Proof` (what you can show, and what is missing), `## Where to use it`.

## reading — `data/readings/<slug>.md`

One reading of the campaign record. `question` (agreed with the user first),
`stage` (`open | supported | contradicted | retired`), `checked` (when the facts
were last re-queried), `created`, optional `area`
(`channels | venues | content | replies | audience | product | pricing | other`),
`impact` (`high | medium | low`) and `impact_estimate` (one line with the
numbers it rests on). **Conditional**: `impact` needs `impact_estimate`. Body:
`## Facts`, `## Reading`, `## Alternatives`, `## Would decide it`, optional
`## Phase`, `## Next step`. Open readings are linked from `data/readings.md`,
the rest from `data/readings-archive.md`.

## reference — `data/references/<slug>.md`

Outside material you take ideas from. `kind`
(`video | article | post | thread | talk | podcast | repo | newsletter | other`),
`stage` (`new | reviewed | used | dropped`), `platform`, `resource`, `author`,
`ideas_for` (one or more of
`content | format | positioning | product | offer | venue`), optional
`published`, `pillar`, `sponsored`, `created`. Body: `## What it is`,
`## Ideas we can use`, `## Source`. Material about your product or your
category's argument is `external` instead.

## note — `data/notes/<slug>.md`

A short durable note. `kind` (`decision | learning | gotcha`), `stage`
(`active | superseded`), `origin` (`human | agent`), `created` (when the fact
came about). **Conditional**: `decision` needs `origin: human`. At most 400
tokens; a title and a paragraph or two.

## weekly-report — `data/weekly/YYYY-MM-DD.md`

A weekly digest kept on file. `window_start`, `window_end`, `created`. Body:
`## Shipped`, `## Engagement`, `## Pipeline and outreach`, `## Next week`.

## hub — indexes and role hubs

A landing page that inclusion-links its members (`data/communities`,
`data/people`, `data/mentions`, `data/interviews`, `data/competitors`,
`data/experiments`, `data/threads`, `data/replies`, `data/leads`, `data/emails`,
`data/audiences`, `data/selling-points`, `data/readings`,
`data/readings-archive`, `data/references`, `data/notes`, `data/weekly`,
`data/people/role-*`, and `data/pillars/*` — one per messaging pillar, created
by setup). `stage: living`, `created`.

## tracker — living reference docs

`data/product`, `data/strategy`, `data/brand`, `data/offer`, `data/metrics`,
`data/website`, `data/plan`, `data/backlog`, this file. `stage: living`.
