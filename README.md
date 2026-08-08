# marketing-workspace

[![OKF
v0.2](https://img.shields.io/badge/OKF-v0.2%20conformant-blue)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)

**Memory for your marketing agent.** A plain-markdown campaign graph — your
positioning, launch venues, people, posts, mentions, plan, and backlog — that
your AI agent reads to decide what to do next and writes back what happened.
Part CRM, part editorial calendar, part system of record.

It is also a conformant [Open Knowledge
Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle — an open standard for agent-maintained knowledge — so your campaign
graph is portable to any OKF consumer, not locked to one tool.

Skill packs like
[marketingskills](https://github.com/coreyhaines31/marketingskills) teach your
agent *how* to do marketing — copywriting, CRO, SEO, launches. But skills are
stateless: every session starts from zero. No record of what was published
where, which venues you've exhausted, who replied to your outreach, or why a
post exists.

This workspace is that record. Install the skills for the knowledge; clone this
for the memory:

- **A campaign graph** (`data/`) — product positioning and messaging pillars,
  **~65 pre-seeded launch venues with posting rules**, a people CRM (customers,
  prospects, amplifiers), the post pipeline (drafts → published, with engagement
  tracking), third-party mentions, a plan, and a backlog.
- **Typed, validated, portable** — every document carries frontmatter checked by
  `iwe schema validate`, so agent writes stay consistent across sessions.
  Provenance (`generated`, `sources`) and lifecycle (`status`, `stale_after`)
  live in frontmatter as OKF families, and CI checks conformance on every
  commit.
- **Workspace skills** (`.claude/skills/`) — the state workflows the packs don't
  have: guided setup, internet backfill, venue-aware drafting, engagement
  refresh, a weekly digest.

Everything is plain markdown in your repo, managed as a knowledge graph by
[IWE](https://iwe.md) — queryable, schema-validated, editor-native, and fully
yours.

Writing code instead of copy? The sibling
[dev-workspace](https://github.com/iwe-org/dev-workspace) template is the same
idea for a software project.

## Quickstart

1. **Use this template** (or clone) and open the folder in your editor.

2. **Install the iwe CLI** (manages the graph, validates the schemas):

   ``` bash
   brew tap iwe-org/iwe && brew install iwe   # Homebrew
   cargo install iwe                          # or via Rust
   ```

   Editor extensions (VS Code, Neovim, Zed) bundle the binaries too — see the
   [quick-start guide](https://iwe.md/quick-start/) for all options.

3. **Optionally, install a marketing skill pack** — recommended:
   [marketingskills](https://github.com/coreyhaines31/marketingskills):

   ``` bash
   npx skills add coreyhaines31/marketingskills -a claude-code
   ```

   (or via Claude Code: `/plugin marketplace add coreyhaines31/marketingskills`,
   then `/plugin install marketing-skills`.) The workspace is fully functional
   without a pack — your agent still plans, drafts, and records against the
   graph. A pack upgrades its marketing *technique*; skip it now and add it
   anytime.

4. **Point your agent at the workspace** — open Claude Code (or any agent CLI
   that reads `AGENTS.md`) in the repo root and say: *"run the setup"*. The
   agent interviews you, fills in `data/product.md` and `data/strategy.md`, and
   prunes the seeded venue list to your ideal customer.

What happens next — the setup interview, the internet backfill, the daily loop —
is described below. No agent? The workspace is still a well-organized markdown
KB you can work by hand.

## How you use it

The workspace is designed to be operated *by the agent, with you*: most of the
work is automated — the agent does the reading, searching, and filing — and you
supply the judgment. A typical start:

**Day one — the interview (~15 minutes).** Say *"run the setup"*. The agent
interviews you about the product, the ideal customer, pricing, and your honest
stage, then writes the foundation docs (`product.md`, `strategy.md`, `brand.md`,
`offer.md`), creates your messaging-pillar hubs, and prunes the ~65 seeded
venues down to the ones that fit your customer. You answer questions and
confirm; it files.

**Day one — the backfill.** Your marketing didn't start today, so have the agent
pull in what already exists. Say *"backfill the workspace"* — the backfill skill
guides it end-to-end — or give any web-capable agent the prompt:

> "Research <product> online and backfill the workspace: read our website and
> fill the gaps in the product doc, find mentions of us on HN, Reddit, and blogs
> and file them, file the posts we've already published with their engagement
> numbers, and stub competitor profiles from their public pages."

It files each find where the graph expects it — mentions with verbatim quotes
and links to their authors, published posts with engagement, competitors with
pricing snapshots — and asks you when it can't verify something. The standing
rule (baked into the skills and `AGENTS.md`): never invent — "unknown" beats
plausible fiction.

**Every session after.** Just talk to the agent; the graph is its memory:

- *"what should I do next?"* — picks from the backlog and the active plan
- *"draft a Show HN post"* — venue rules + positioning + pack technique, filed
  as a draft with its goal recorded
- *"log this mention: <url>"* / *"I published it, here's the link"*
- *"we talked to a churned customer, here are my notes"* — filed as an
  interview, best quotes distilled into the product doc
- *"weekly digest"* — what shipped, what moved, what to do next week

Every action leaves a record in `data/`, so the next session — or a different
agent entirely — picks up exactly where this one left off. You stay in the loop
where judgment matters: confirming positioning, approving venue cuts, and giving
the final yes before anything is published.

## How the two halves compose

| Half          | Provides                             | Lives                         |
| ------------- | ------------------------------------ | ----------------------------- |
| **Knowledge** | marketing technique (the skill pack) | `.claude/skills/` (installed) |
| **Memory**    | your campaign's state (this repo)    | `data/`                       |

The knowledge half is optional and swappable: we deliberately don't bundle a
pack (design rationale in `STRUCTURE.md`) — install whichever one you like, or
none. The memory half carries the workspace on its own.

Example: *"draft a Show HN post."* The agent takes positioning from
`data/product.md`, posting rules from `data/community-hn.md`, copy technique
from the skill pack — then files the draft in `data/drafts/hn/`, links it from
the venue, and records `goal` and `success` so future sessions know why it
exists.

The integration is native: skill packs like marketingskills read
`.agents/product-marketing.md` for product context before asking you anything.
The setup skill **generates that file from your workspace** (product doc + brand
voice + current offer), so every skill in the pack starts pre-briefed — and the
workspace's experiment log, metrics baselines, and competitor profiles answer
the questions packs otherwise re-ask every session ("what have you tried?",
"what's your conversion rate?", "who are your competitors?").

The graph is queryable:

``` bash
iwe find --filter '{type: community, category: launch-platform, stage: planned}'  # where to launch next
iwe find --filter '{type: post, stage: draft}' -f keys                            # drafts in flight
iwe find --filter '{type: person, stage: contacted}'                              # outreach awaiting reply
iwe find --filter '{type: task, stage: planned, priority: high}'                  # what to do next
iwe schema validate                                                                # is every doc well-formed?
```

## What's inside

```
data/
  product.md       positioning, ICP, personas, objections, customer language  ← start here
  strategy.md      current stage (first 10 / 100 / 1000), channels, goals
  brand.md         voice, banned words, description variants, asset locations
  offer.md         tiers, value metric, guarantee, pricing change history
  metrics.md       baselines: unit economics, funnel, audience, authority
  website.md       page inventory, keyword map, org facts, redirect ledger
  pillars/         one hub per messaging pillar — links every post carrying it
  community-*.md   ~65 seeded launch venues with posting rules
  communities/     platform-grouped venues (reddit/ holds the 28 subreddits)
  competitors/     one source-of-truth profile per competitor, dated snapshots
  experiments/     everything tried, with hypothesis, verdict, and learning
  people/          your CRM: customers, prospects, amplifiers
  drafts/ posts/   the editorial pipeline, engagement tracked in frontmatter
  evergreen/       reusable platform-independent post templates
  mentions/        third-party mentions of your product
  interviews/      customer research records: quotes, learnings, per interview
  plan/ backlog/   campaign moves and atomic tasks
.claude/skills/    setup · backfill · draft · update · weekly
.iwe/              graph config + frontmatter schemas
```

`SCHEMA.md` documents every document type; `AGENTS.md` is the agent's operating
manual.

## Built on IWE

[IWE](https://iwe.md) is open-source, local-first knowledge-graph tooling for
markdown: a CLI (`iwe`) and an LSP server that plug into VS Code, Neovim, Zed,
and Helix. This workspace needs only the CLI — that's what powers the queries,
renames, and schema validation above — but with the editor extension installed,
the whole campaign graph gets IDE ergonomics: follow links, see backlinks
("which venues link this post?"), rename entities safely, and run extract/inline
refactors on your docs.

- Website & docs: [iwe.md](https://iwe.md) · [quick
  start](https://iwe.md/quick-start/)
- Source: [github.com/iwe-org/iwe](https://github.com/iwe-org/iwe)

## Acknowledgments

Designed to pair with Corey Haines'
[marketingskills](https://github.com/coreyhaines31/marketingskills). Venue
discovery was guided by Edoardo Stradella's
[Marketing-for-Founders](https://github.com/EdoStra/Marketing-for-Founders) — a
great human-readable collection; go star both. Built on [IWE](https://iwe.md).

## License

MIT — see [LICENSE.md](LICENSE.md).
