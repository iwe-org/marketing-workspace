# Structure

Where everything in this workspace lives. The [agent manual](AGENTS.md) says how
to work in it and [Schema](SCHEMA.md) lists the fields of each document type.

## Layout

```
index.md                the start page
AGENTS.md               the agent operating manual
SCHEMA.md               frontmatter reference for every type
STRUCTURE.md            this page
.iwe/
  config.toml           workspace config and the schema bindings, by path
  schemas/              one schema per type (23 types and 3 OKF checks), and a
                        *.ui.yaml display sidecar for each type
.claude/skills/         setup, backfill, draft, update, weekly, iwe
data/                   the campaign graph
  index.md              the graph root: link bullets to every hub
  log.md                dated history of state changes
  product.md            positioning, ICP, personas, objections, pillars      (to fill in)
  strategy.md           current stage, channels, goals, cadence              (to fill in)
  brand.md              voice, description variants, visual kit, assets      (to fill in)
  offer.md              tiers, guarantee, pricing feedback, change history   (to fill in)
  metrics.md            dashboard and baselines                              (to fill in)
  metrics-weekly.csv    one row per week; the metrics and plan charts read it
  website.md            pages, keyword map, organization facts               (to fill in)
  communities.md        the venue hub, grouped by category
  community-*.md        39 seeded standalone venues, `stage: planned`
  communities/reddit/   28 seeded subreddits
  pillars.md + pillars/ one hub per messaging pillar, created by setup
  drafts/<platform>/    unpublished posts
  posts/<platform>/     published posts; posts.md charts them
  evergreen/            reusable post templates
  people.md + people/   the CRM, with a role-*.md hub per role
  competitors.md + competitors/
  experiments.md + experiments/
  mentions.md + mentions/        third-party mentions, and the watchlist
  interviews.md + interviews/    customer interviews and surveys
  threads.md + threads/          threads where you met people
  replies.md + replies/          comments you posted in those threads
  leads.md + leads/              people met in threads
  emails.md + emails/            outreach email
  audiences.md + audiences/      audience segments
  selling-points.md + selling-points/
  readings.md + readings/        open readings; readings-archive.md holds the rest
  references.md + references/    outside material to take ideas from
  notes.md + notes/              decisions, learnings, gotchas
  weekly.md + weekly/            weekly digests kept on request
  plan.md + plan/                three seeded stage plans
  backlog.md + backlog/          eight seeded onboarding tasks
```

On Reddit the `drafts/` and `posts/` platform folder nests one level deeper, per
subreddit: `data/posts/reddit/<subreddit>/`.

## What is seeded

- **67 venues**: 39 standalone sites and 28 subreddits, all `stage: planned`.
  Each carries notes on how to post there, researched in July 2026; 20 of them
  say "rules unverified — check before submitting". Setup re-reads the rules of
  the venues you keep.
- **Three stage plans** in `data/plan/` — first 10, first 100 and first 1000
  users, each with `growth_stage` set — and **eight onboarding tasks** in
  `data/backlog/`.
- **Example documents**: every file named `*-example.md` belongs to one
  fictional product, PlainInvoice, and shows a directory's document shape and
  fills the charts. Setup deletes them all.

## Skills

| Skill      | Reads                                 | Writes                                                              |
| ---------- | ------------------------------------- | ------------------------------------------------------------------- |
| `setup`    | product.md gaps, seeded backlog       | product.md, strategy.md, pillar hubs, venue pruning                 |
| `backfill` | the web: site, HN/Reddit, competitors | mentions, posts, people, competitor docs                            |
| `draft`    | product.md, venue rules, pack skills  | data/drafts/<platform>/*, venue `## Posts`; threads, leads, replies |
| `update`   | published posts' and replies' URLs    | `engagement.*`, reply `outcome`, post `verdict`                     |
| `weekly`   | engagement, replies, plan, backlog    | `data/weekly/` on request                                           |
| `iwe`      | —                                     | the `iwe` CLI reference                                             |
