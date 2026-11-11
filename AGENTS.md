# Agent operating manual

You are operating a **marketing workspace**: a markdown knowledge graph that is
the campaign's memory and system of record. The division of labor:

- **Marketing technique** comes from an installed skill pack (e.g.
  [marketingskills](https://github.com/coreyhaines31/marketingskills)) —
  copywriting, CRO, SEO, launch strategy. Use those skills whenever the task is
  *how to market*. **A pack is optional**: if none is installed, proceed with
  your own marketing judgment plus the venue notes — the workspace functions
  fully either way. Suggest the install once when it would clearly help; never
  block or nag.
- **Campaign state** lives here, in `data/` — positioning, venues, people,
  leads, posts, replies, mentions, readings, plan, backlog. Every action you
  take must leave a record in the graph; this is your memory across sessions.
  Never keep campaign state only in conversation.

## Start of every session

1. Read `data/product.md` and `data/strategy.md`. If they are unfilled, run the
   setup flow (`.claude/skills/setup/SKILL.md`) before anything else — marketing
   work without positioning is guessing.
2. Check the backlog and plan:
   `iwe find --filter '{type: task, stage: planned, priority: high}'` and
   `iwe find --filter '{type: plan, stage: in-progress}'`.
3. Read the active decision notes
   (`iwe find --filter '{type: note, kind: decision, stage: active}'`) — they
   are choices the user already made — and list the replies due their review:
   `iwe find --filter '{type: reply, outcome: {$exists: false}}'`, any published
   three or more days ago.

## The operating loop

1. **Pick** the next action from the backlog (or the user's request).
2. **Consult** before acting: the venue entity (`data/community-<slug>.md` for
   standalone sites, `data/communities/reddit/<name>.md` for subreddits —
   `notes` carry posting rules and flair requirements), `data/product.md` for
   positioning, and the matching skill-pack skill for technique.
3. **Execute** — draft the post, prepare the outreach, file the research.
4. **Record** — write the state back:
   - New draft → `data/drafts/<platform>/YYYY-MM-DD-<slug>.md` (`type: post`,
     `stage: draft`), plus a link under the venue's `## Posts` heading and an
     inclusion link from its pillar hub (`data/pillars/<pillar>.md`).
   - Published → `iwe rename` the draft from `data/drafts/...` to
     `data/posts/...`, set `stage: published`, `published`, `resource`.
   - New contact/reply → update the person's entry, or create one in
     `data/people/`.
   - Comment posted in someone else's thread → file in this order: the thread
     page `data/threads/<platform>-<slug>.md` (links the venue), a lead
     `data/leads/<handle-slug>.md` if the person it answered may want the
     product, then the reply `data/replies/YYYY-MM-DD-<platform>-<slug>.md`
     whose `## Context` opens with a `Person:` link to the lead or person.
     Inclusion-link the reply from its thread page and from `data/replies.md`.
     Three days later, set `outcome` and `reviewed`; raise the outcome whenever
     the conversation moves.
   - Lead converts (tries the product, or a real relationship starts) → create
     their `data/people/` record, link it from the lead, set the lead's
     `stage: converted`, and move its link in `data/leads.md`.
   - Outreach email drafted or sent → `data/emails/YYYY-MM-DD-<slug>.md` linking
     the recipient inline; flip `stage` to `sent` with `sent` set, and update
     the person's `stage`.
   - Mention spotted → `data/mentions/YYYY-MM-DD-<slug>.md` + link from
     `data/mentions.md` and the venue's `## Mentions` heading.
   - Interview or survey done → `data/interviews/YYYY-MM-DD-<slug>.md` (`kind`,
     notes, verbatim quotes; link the interviewee's person doc inline), then
     distill the best language into `data/product.md`.
   - Launch plan step finished → append a `## Retro` section (numbers, what
     worked) before closing it.
   - Venue worked → flip its `stage` (`planned` → `active`, or `cancelled` with
     a note).
   - Task finished → set `stage: done` and `completed` on the task doc, and move
     its link in `data/backlog.md` to the `## Done` section.
   - Something tried (a test, a price change, a new popup) →
     `data/experiments/<slug>.md` with hypothesis and metric; on conclusion set
     `verdict` and `result`. Before proposing any experiment, check what's
     already been tried:
     `iwe find --filter '{type: experiment}' --project '$title,stage,verdict'`.
   - Competitor fact learned (pricing change, new positioning) → update
     `data/competitors/<slug>.md`, dated entry in its Changelog, bump `updated`.
     Set `threat` only with the reason written in the body.
   - Published post's success line checked → set `success_evidence`, `verdict`
     and `verdict_date`, comparing the evidence to the line as written.
   - The user asks what the record means (why a venue went quiet, whether a kind
     of post works) → a reading at `data/readings/<slug>.md`: agree the
     `question` first, then the facts with the queries that produced them, the
     reading, at least one alternative, what would decide it, and a next step
     that can be undone. Link it from `data/readings.md` by `impact`; when its
     `stage` leaves `open`, move the link to `data/readings-archive.md`.
   - Reason to buy noticed (a customer's line, a workflow that sells) →
     `data/selling-points/<slug>.md` with its `evidence` and what `## Proof`
     still lacks. A `hypothesis` point never goes into copy as a claim.
   - Outside material worth learning from (how someone else launched, a format
     that works) → `data/references/<slug>.md` with the ideas written down; set
     `stage: used` and link where an idea went.
   - Audience segment defined or ruled out → `data/audiences/<slug>.md`; an
     `inactive` segment keeps its reason in `notes`.
   - Decision the user made, a durable learning, or a gotcha that cost time → a
     short note at `data/notes/<slug>.md`. Record `kind: decision` only for what
     the user stated or confirmed (`origin: human`); a note replaced by a newer
     one is set `superseded`, never deleted.
   - Baseline number learned → `data/metrics.md` (current value + dated
     snapshot). Weekly numbers (visits, signups, activated, paying users, MRR)
     are one row per week in `data/metrics-weekly.csv`; append the row rather
     than editing past ones.
   - Positioning/messaging changed → update `data/product.md` **and log it in
     its Changelog**, then regenerate `.agents/product-marketing.md` (see
     below).
5. **Stamp** — every document you create or meaningfully change gets
   `generated: { by: claude-code/opus-5, at: <ISO 8601 now> }`, a one-sentence
   `description` if it has none, and — when you researched it from an external
   page — a `sources` entry with that page's `resource`. Whenever you set
   `stage`, derive OKF `status` from the table in `SCHEMA.md` and set or clear
   it in the same edit. Append a line to `data/log.md` under today's
   `## YYYY-MM-DD` group (create the group if it isn't there); if the top-level
   set of documents changed, update `data/index.md` too.
6. **Validate & commit** — `iwe schema validate` must pass; then commit with a
   short message describing the state change.

## Conventions

- **Inclusion link** = a markdown link on its own line — it makes the target a
  child in the graph. Hubs (`data/communities.md`, `data/plan.md`, …)
  inclusion-link their members. Inline links (inside sentences/list items) are
  soft references.
- **A post's venue is a relationship, not a field**: the link lives under the
  community file's `## Posts` heading. Query it:
  `iwe find --referenced-by data/community-<slug> --filter 'type: post'`
  (subreddits: `--referenced-by data/communities/reddit/<name>`).
- **A person's role is a relationship**: inclusion links from
  `data/people/role-*.md` hubs (customer, prospect, amplifier, creator,
  contact). Query: `iwe find --included-by data/people/role-customer`.
- **A post's pillar is both a field and an edge**: the `pillar` field is what
  you filter on; the inclusion link from `data/pillars/<pillar>.md` is what
  makes the post reachable when browsing the tree. Set both when filing a draft.
- **Every publication states `goal`** (awareness | conversion | positioning |
  credibility | retention) **and `success`** (the observable signal that means
  it worked). If you can't name them, the post isn't worth planning.
- **Dated filenames**: posts, mentions, interviews, replies, and emails are
  `YYYY-MM-DD-<slug>.md`, kept weekly reports `YYYY-MM-DD.md`; entities (people,
  leads, threads, venues, tasks) are undated slugs.
- **In other people's threads the user writes the words.** You find threads
  worth answering, read them, and draft talking points and accuracy checks; the
  user writes and posts the comment. Record who wrote it in the reply's
  `origin`. A reply speaks for a person, and readers notice a drafted voice.
- **Each fact is written once**, on the record made when it happened. A lead
  does not store whether they answered or how many replies they got — that is
  read from the replies that link to them. A reply does not store its thread —
  the thread page inclusion-links it.
- **Facts and readings stay apart.** A count, a quote or a date is a fact; what
  it means is a reading. Before a reading becomes a rule, write down another
  explanation and the evidence that would tell them apart.
- **Closed items move, they are not deleted.** Hubs list what is open; finished
  or decided items move to a closing section or an archive hub — the backlog's
  `## Done`, the leads hub's `## Converted` and `## Dropped`,
  `data/readings-archive.md`.
- **Repurposing trail**: a dated post cut from an evergreen template links back
  to the template inline, so a template's real-world performance stays
  traceable.
- **Example docs**: files suffixed `-example.md` demonstrate each directory's
  document shape (a fictional product; schema-validated so they can't rot).
  Ignore them when reporting real campaign state; the setup skill deletes them
  at the end of onboarding.
- **Frontmatter shapes are enforced** — `.iwe/schemas/*.yaml`, human reference
  in `SCHEMA.md`. Run `iwe schema validate -k <key>` after editing a doc; fix
  violations before committing.
- **`data/` is an OKF v0.2 bundle** — the graph is portable knowledge any [Open
  Knowledge
  Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  consumer can read, and CI checks conformance on every commit. Three rules keep
  it true: every document under `data/` has frontmatter with a non-empty `type`;
  `data/index.md` carries no frontmatter beyond `okf_version` and stays sections
  of link bullets; `data/log.md` stays date-grouped bullets under
  `## YYYY-MM-DD`. `okf.yaml`, `okf-index.yaml`, and `okf-log.yaml` enforce all
  three — never work around them by unbinding a schema.
- **Charts read the records.** Hubs carry ```` ```chart ```` blocks — a
  Vega-Lite spec whose data is a query over the graph or a data file — so the
  numbers on them are never typed by hand. When you file a record, the charts
  update on their own; don't copy their numbers into prose. Two habits keep them
  true: every post, mention and reply links its venue page once in the body (the
  venue charts count those links), and every reply's `## Context` opens with the
  `Person:` line (the leads charts match it by handle).
- **Data files.** Measurements that aren't documents — weekly metrics, a funnel
  export — live as CSV, TSV, JSON or JSONL files under `data/`, beside the page
  that charts them (`data/metrics-weekly.csv`). Keep a header row, one row per
  period, ISO dates; a chart's `data: {url: <file>}` reads it.
- **Links carry `.md`** — `refs_extension = ".md"`, so a link resolves for
  readers outside iwe. Run `iwe normalize` after bulk edits rather than hand-
  writing link targets.
- **The `.agents/product-marketing.md` bridge**: marketing skill packs read that
  file for product context before asking questions. It is *generated* from
  `data/product.md` (+ brand voice/descriptions, current offer) by the setup
  skill — never edit it directly; edit the source docs and regenerate.

## IWE basics

The graph is managed by [IWE](https://iwe.md) — the `iwe` CLI. What you must
know:

- A document's **key** is its extension-less path relative to the repo root
  (`data/product`, `data/posts/hn/2026-08-01-launch`) — that's what `-k` and the
  structural flags take.
- A document's title resolves from its H1 header (or a `title` frontmatter field
  where the H1 doesn't carry it).
- **Never `mv` or hand-delete a document** — use `iwe rename` / `iwe delete`,
  which update every link in the graph; a plain `mv` silently breaks references.
  After a rename or delete, check `git diff`: the reference updates are part of
  the change, and the diff is how you verify none flattened or went dangling.
- If `iwe` isn't installed (command not found): the workspace is still plain
  markdown — reading and editing work fine — but renames, queries, and
  validation need the CLI. Ask the user to install it
  (https://iwe.md/quick-start/) before restructuring anything.

## IWE CLI cheatsheet

``` bash
iwe find --filter '{type: community, stage: planned}' -f json   # filter docs (YAML expr)
iwe find --filter 'stage: draft' -f keys                        # keys only
iwe count --filter '{type: post, stage: published}'             # count matches
iwe retrieve -k data/product                                     # read a doc
iwe tree -k data/index                                           # graph overview
iwe update -k <key> --set stage=published --set 'published="2026-08-01"'
iwe rename data/drafts/reddit/x data/posts/reddit/x              # move; references auto-update
iwe schema validate                                              # validate all bound docs
```

Filters are YAML (`$eq`, `$ne`, `$in`, `$gte`, `$exists`, …), not jq. Structural
anchors: `--includes`, `--included-by`, `--references`, `--referenced-by`,
`--roots`.

## Workspace skills

State workflows (distinct from the skill pack's technique skills):

| Skill                              | What it does                                                       |
| ---------------------------------- | ------------------------------------------------------------------ |
| `.claude/skills/setup/SKILL.md`    | Onboarding interview → fills product.md/strategy.md, prunes venues |
| `.claude/skills/backfill/SKILL.md` | Imports existing mentions, posts, competitor facts from the web    |
| `.claude/skills/draft/SKILL.md`    | Drafts a post, or talking points for a thread reply, then files it |
| `.claude/skills/update/SKILL.md`   | Refreshes engagement on published posts and replies                |
| `.claude/skills/weekly/SKILL.md`   | Prints a weekly digest; keeps it in `data/weekly/` when asked      |
