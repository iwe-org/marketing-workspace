# Structure

This document explains how the workspace is put together and why — the design
rationale behind the layout. The README carries the user-facing story.

## What this is

**marketing-workspace** — the **memory half** of an agent-run marketing
operation, for founders. A plain-markdown campaign graph — positioning, venues,
people, leads, posts, replies, mentions, readings, plan, backlog — with typed,
schema-validated frontmatter, managed by [IWE](https://iwe.md).

It deliberately does **not** ship marketing know-how. Skill packs already do
that well — [marketingskills](https://github.com/coreyhaines31/marketingskills)
(MIT, Corey Haines) is the recommended companion, and this workspace **extends**
it rather than competing:

| Half          | What it provides                                       | Where it lives            |
| ------------- | ------------------------------------------------------ | ------------------------- |
| **Knowledge** | how to do marketing (copywriting, CRO, SEO, launches…) | installed skill pack      |
| **Memory**    | what *you* did, know, and should do next               | this repo's `data/` graph |

Skill packs are stateless: every session starts from zero — no record of what
was published where, which venues are exhausted, who replied to outreach, or why
a post exists. This workspace is that record: part CRM, part editorial calendar,
part system of record, readable and writable by the agent across sessions.

In short: install marketingskills for the knowledge; use this workspace for the
memory.

## Sources & licensing

- Venue seeds (~65 `data/community-*.md`) were *discovered via*
  [Marketing-for-Founders](https://github.com/EdoStra/Marketing-for-Founders)
  (CC BY-SA 4.0) but are an independent compilation: each venue rebuilt from its
  primary source (liveness check, posting rules read at the venue where
  fetchable), with its own annotations, selection, and grouping. No prose or
  wholesale curation is copied.
- marketingskills is referenced as an install, never vendored — no license
  entanglement.
- This repo ships **MIT**; acknowledgments live in the README.

## Layout

One IWE project rooted at the repo root:

```
marketing-workspace/
├── README.md               # human quickstart
├── AGENTS.md               # agent operating manual (portable convention)
├── index.md                # the start page the iWe app opens first
├── LICENSE.md              # MIT
├── SCHEMA.md               # frontmatter reference for all types
├── STRUCTURE.md            # this design doc
├── .iwe/
│   ├── config.toml         # frontmatter_document_title + [schemas] bindings
│   └── schemas/            # machine-checked shape per type (23 types + 3 OKF
│                           #   checks), and a *.ui.yaml table view per browsable type
├── .claude/skills/         # workspace skills (state workflows, not marketing technique)
│   ├── setup/SKILL.md      # onboarding interview → fills product.md, prunes venue seeds
│   ├── backfill/SKILL.md   # imports existing mentions/posts/competitor facts from the web
│   ├── draft/SKILL.md      # draft a post for a venue (positioning + venue rules + pack skills)
│   ├── update/SKILL.md     # engagement refresh on published posts
│   ├── weekly/SKILL.md     # weekly digest
│   └── iwe/SKILL.md        # the iwe CLI reference, read on demand
└── data/                   # the campaign graph (agent reads AND writes)
    ├── index.md            # KB root
    ├── product.md          # FILL-IN: 11-section positioning/ICP/personas/objections/
    │                       #   customer-language/switching-forces doc with changelog (tracker)
    ├── strategy.md         # FILL-IN: current stage, chosen channels, goals (tracker)
    ├── brand.md            # FILL-IN: voice, description variants, visual kit, assets (tracker)
    ├── offer.md            # FILL-IN: tiers, value metric, guarantee, change history (tracker)
    ├── metrics.md          # FILL-IN: baselines + dated snapshots (tracker)
    ├── website.md          # FILL-IN: page inventory, keyword map, org facts, redirects (tracker)
    ├── communities.md      # hub, grouped: launch platforms / directories / deals / subreddits
    ├── community-*.md      # SEEDED standalone venues, stage: planned, with rule notes
    ├── communities/reddit/ # SEEDED subreddits (platform-grouped, mirrors posts/<platform>/)
    ├── competitors.md + competitors/   # one source-of-truth profile per competitor
    ├── experiments.md + experiments/   # everything tried: hypothesis, verdict, learning
    ├── people.md + people/ # CRM: customers, prospects, amplifiers + role hubs
    ├── pillars/            # one hub per messaging pillar (setup creates; posts
    │                       #   inclusion-linked from here — keeps them reachable)
    ├── posts/<platform>/   # published
    ├── drafts/<platform>/  # unpublished
    ├── evergreen/          # reusable template posts
    ├── mentions.md + mentions/   # third-party mention tracking (+ listening watchlist)
    ├── interviews.md + interviews/   # deliberate research records (customer interviews, surveys)
    ├── threads.md + threads/     # threads where you met people; inclusion-link their replies
    ├── replies.md + replies/     # comments you posted in others' threads, judged by outcome
    ├── leads.md + leads/         # people met in threads; graduate to people/ on conversion
    ├── emails.md + emails/       # outreach email, sent and received
    ├── audiences.md + audiences/ # one audience segment each
    ├── selling-points.md + selling-points/   # reasons to buy, with proof
    ├── readings.md + readings/   # what the record means — open readings by impact
    ├── readings-archive.md       # readings that left `open`, by verdict
    ├── references.md + references/   # outside material to take ideas from
    ├── notes.md + notes/         # short decisions, learnings, gotchas
    ├── weekly.md + weekly/       # weekly digests kept on request
    ├── plan.md + plan/     # campaign moves — SEEDED with three stage plans
    └── backlog.md + backlog/     # atomic tasks — SEEDED with onboarding checklist
```

## The skill-pack enrichment (2026-07-26)

We read all 48 marketingskills SKILL.mds and mapped what persistent state each
would read/write. Two findings drove the current shape:

1. **The bridge**: every pack skill reads `.agents/product-marketing.md` before
   asking the user anything (the pack's own `product-marketing` skill maintains
   it — 12 sections, versioned changelog). Our `data/product.md` was deepened to
   parity (personas/anti-persona, objections, verbatim customer language,
   switching forces, competitive landscape, changelog) and the setup skill
   **generates** the bridge file from it + brand voice + current offer. Edit the
   source, regenerate the export.
2. **The recurring re-asks**: skills constantly re-request baselines ("what's
   your conversion rate?"), history ("what have you tried?"), and competitor
   facts. Hence `data/metrics.md` (~20 skills), `type: experiment` (~17),
   `type: competitor` (~8, two skills literally demand a centralized
   per-competitor source of truth), `data/brand.md` (~7, incl. the tier-keyed
   description-variant library), and `data/website.md` / `data/offer.md` (~6
   each).

Deferred to v2 (analysis on file): a full VOC quote-bank type (problem-space
customer language beyond `mention`'s about-us scope — started as product.md
sections), per-person outreach thread history, a sequence/owned-channel
registry, asset/collateral inventory, AI-visibility query tracker, milestones
timeline, loop run-state.

## The Marketing-for-Founders second pass (2026-07-26)

A re-read of the source syllabus against the grown structure surfaced three
adoptions beyond the original venue/stage mining: **`type: interview`** (its
User Research section is its densest — four interview scripts — and our own
first-10 plan demanded "file everything you learn" with nowhere to file it;
interviews are the deliberate-research counterpart to `mention`'s spontaneous
quotes, and the feeder for product.md's customer-language/objections sections),
the **listening watchlist** on `data/mentions.md` (the input side of social
listening — what we track, where, with which alert tool), and the **launch-retro
convention** on plan steps (launches recur; the next one should start from
evidence). Minor: repurposing back-links (post → evergreen template), and the
GitHub-README-as-landing-page note in website.md.

## Decision: skill packs are optional companions — no vendoring (2026-07-26)

We considered vendoring marketingskills into this repo (copying the 48 skills
in, or a git submodule) and decided against both. The rationale, for the record:

- **We extend, we don't absorb.** The project's positioning — and its best
  launch asset — is a genuine partnership posture toward the skill-pack
  ecosystem. A repo that is 80% someone else's skills reads as a fork, not a
  companion, and burns the goodwill of the exact people best placed to amplify
  it. Send installs and stars upstream.
- **No technical need.** Both integration points degrade gracefully: the
  `.agents/product-marketing.md` bridge is useful to any pack (or none), and the
  draft skill delegates to a technique skill only *if present*. The workspace is
  fully functional standalone — a pack upgrades the agent's marketing technique;
  it does not gate the memory.
- **Maintenance without payoff.** Upstream ships breaking waves (v2.0 renamed 17
  skills); vendored copies plus local adaptations would make every sync a
  48-file merge. And template copies are disconnected from us after "Use this
  template" anyway, so vendoring buys freshness for nobody.
- **Duplicate-skill confusion.** Users who also install upstream would carry two
  copies with overlapping names — the exact stale-folder mess upstream's own
  upgrade guide warns about.
- **Submodules specifically**: wrong nesting (their skills live at
  `skills/<name>/`, not where Claude Code discovers them) and a
  clone-without-`--recursive` trap for non-git founders. A pinned pointer that
  doesn't work as an install.

Operationally: the setup skill offers the install once (with the
`-a claude-code` caveat handled) and proceeds without complaint if declined; the
README states the pack is optional. MIT keeps the vendoring door open forever —
if upstream ever goes unmaintained or a rename wave breaks the bridge, a pinned
snapshot is one commit away, with an honest "preserved because upstream went
quiet" story.

## The growth-stage axis

The "first 10 / first 100 / first 1000 users" framing survives the pivot as a
**data** concept (not a knowledge hub): `data/strategy.md` records the current
stage, the `plan` schema has a `stage` field, and the three seeded plan steps
carry the stage guidance in their bodies ("nothing at this stage scales, and
that's the point", etc.).

## Data-layer design

Types: `post`, `template` (evergreen), `community`, `person`, `mention`,
`external`, `task`, `plan`, `hub`, `tracker`. Kept axes: `goal` + `success` on
every publication (awareness/conversion/positioning/credibility/retention);
venue-as-relationship (`## Posts` / `## Mentions` under the community file);
roles-as-hubs
(`data/people/role-{customer,prospect,amplifier,creator,contact}`).

Design choices:

- **Pillars are founder-defined** — `pillar` is a free string validated only for
  presence; `data/product.md` instructs defining 3–5 and the setup skill fills
  them in, creating a hub per pillar at `data/pillars/<slug>.md`. Every
  publication is inclusion-linked from its pillar hub — without that edge, posts
  would all be graph roots, reachable by query but not by browsing the tree.
- **`community` gains `category`**
  (`launch-platform | directory | deals | subreddit | forum | community | social | newsletter | other`)
  — the checklist axis the agent filters on; `platform` becomes a loose string.
- **`person` is CRM-flavored** — adds `email`, drops the influence-score rubric.
- **Dropped for v1**: `discussion`, `feedback-report`, `weekly-report`,
  `submission`, `audience` types; directory submissions are `task`s against
  seeded directory venues. (1.2.0 brought back `audience` and `weekly-report`;
  see below.)
- **Seeds**: ~65 venues (`stage: planned`), three stage plan steps, onboarding
  backlog ("Fill in product.md", "Define pillars", "Prune venues to your ICP",
  "Pick your first launch venue").

## The conversation and evidence pass (1.2.0)

Lessons from running a campaign workspace day to day, carried over as generic
types and rules:

- **Replies are their own records.** Comments posted in other people's threads
  were the most frequent action and had nowhere to live. They now form a small
  graph: `thread` (where), `reply` (what was said and how it landed), `lead`
  (who may want the product). Each fact is written once, on the record made when
  it happened — a lead stores no reply count or "answered" flag, because those
  drifted from the replies themselves; they are read from the replies' links.
- **Outcome over score.** A reply is judged by what it started (`none` →
  `replied` → `asked` → `will-try` → `tried`), set at a three-day review.
  Upvotes stay as context. `names_product` and `origin` are recorded so outcomes
  can be compared with and without the pitch, and by who wrote the words.
- **The user writes the words in others' threads.** Drafted comments read as
  drafted; the agent's job there is finding, reading, talking points and filing.
- **Facts apart from readings.** A `reading` keeps observations (with the
  queries that produced them) apart from what they are taken to mean, names an
  alternative, and says what would decide it. Readings that leave `open` move to
  an archive hub — the general pattern for closed items: hubs list what is open.
- **Success lines get checked.** Posts gained `success_evidence`, `verdict` and
  `verdict_date`, so "did it work?" is answered against the line written before
  publishing.
- **Arguments before copy.** `selling-point` records each reason to buy with its
  `evidence`; a `hypothesis` cannot be marked as carried by copy.
- **Conditional requirements** replace conventions that were only written down:
  a done task carries `completed`, a concluded experiment its verdict and
  result, a decision note `origin: human`, an outbound email its goal.
- **Ratings need reasons.** Competitor `threat` is optional and set only with
  the reason in the body; unexplained ratings proved worse than none.

**Charts over the records.** The replies, leads, mentions, communities,
experiments, plan, metrics and weekly hubs, and a new `data/posts.md` page,
carry ```` ```chart ```` blocks: Vega-Lite specs whose data is a query over the
graph or a CSV beside the notes (`data/metrics-weekly.csv`), composed with
shared `datasets`, `lookup` joins and `hconcat` stat tiles. About eighty dummy
`*-example.md` records spread over May to early August fill them, so a fresh
workspace shows what each chart is for. Two notes on the joins:

- In 1.2.0 the examples were named `*.example.md`, and a link to one was not a
  graph edge: the final `.example` read as a file extension, so iwe treated the
  link as a link to a file. 1.2.1 renamed them `*-example.md`, which makes the
  links edges, but the charts still use edges only to the seeded venue pages
  (`$referencedBy` on a community), and key prefixes and fields everywhere else.
- The leads tiles join each reply to its lead by the handle on the reply's
  `Person:` line, read from `$content`, rather than by the link, for the same
  reason.

Also added: `email`, `audience`, `reference` (outside craft material, distinct
from `external`, which is about you or your thesis), `note`, `weekly-report`,
competitor `relation`/`watch`, experiment `stop_signal`, community
`members`/`checked`, and a `*.ui.yaml` table view for every type a person
browses. Still left out: synced platform discussions, per-product docs (one
product per workspace), directory `submission` (a task against a venue), and
anything that needs a script.

## Workspace skills (five workflows and a reference)

State workflows, deliberately disjoint from marketingskills' technique skills.
They live at `.claude/skills/` (Claude Code auto-discovers; other agents reach
them via AGENTS.md).

| Skill      | Reads                                 | Writes                                                              |
| ---------- | ------------------------------------- | ------------------------------------------------------------------- |
| `setup`    | product.md gaps, seeded backlog       | product.md, strategy.md, venue pruning                              |
| `backfill` | the web: site, HN/Reddit, competitors | mentions, posts, people, competitor docs                            |
| `draft`    | product.md, venue rules, pack skills  | data/drafts/<platform>/*, venue `## Posts`; threads, leads, replies |
| `update`   | published posts' and replies' URLs    | `engagement.*`, reply `outcome`, post `verdict`                     |
| `weekly`   | engagement, replies, plan, backlog    | `data/log.md`; `data/weekly/` on request                            |

`draft` is the composition point: it pulls positioning from `data/product.md`,
posting rules from the venue's `notes`, and — when marketingskills is installed
— delegates copy technique to the matching pack skill, then files the result
into the graph.
