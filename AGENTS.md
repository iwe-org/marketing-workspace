# Agent operating manual

You are operating a **marketing workspace**: a markdown knowledge graph that is
the campaign's memory and system of record.

- **Campaign state** lives here, in `data/` — positioning, venues, people,
  leads, posts, replies, mentions, readings, plan, backlog. Every action you
  take must leave a record in the graph; this is your memory across sessions.
  Never keep campaign state only in conversation.
- **Marketing technique** is your own judgment plus the venue notes. If the user
  has installed a marketing skill pack (e.g.
  [marketingskills](https://github.com/coreyhaines31/marketingskills)), use its
  skills when the task is *how to market*. A pack is optional and the user's to
  install: never install one yourself, and never block on one.

## Start of every session

1. Read `data/product.md` and `data/strategy.md`. The workspace is **set up**
   when four sections of `data/product.md` have content: What is it, Ideal
   customer profile, Positioning and Messaging pillars. A section marked
   `✏️ skipped <date>` is the user's decision to leave it for later: it counts
   as dealt with, does not trigger setup and does not block drafting. If one of
   the four still shows a bare ✏️, offer the setup flow
   (`.claude/skills/setup/SKILL.md`) before other marketing work — work without
   positioning is guessing.
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
   `notes` carry posting rules and flair requirements) and `data/product.md` for
   positioning.
3. **Execute** — draft the post, prepare the outreach, file the research.
4. **Record** — write the state back, using the list below.
5. **Stamp** — every document you create or meaningfully change gets
   `generated: { by: <your agent>/<your model>, at: <ISO 8601 now> }`, naming
   the agent and model you are actually running as (e.g. `claude-code/opus-5`),
   a one-sentence `description` if it has none, and — when you researched it
   from an external page — a `sources` entry with that page's `resource`.
   Whenever you set `stage`, derive OKF `status` from the table in `SCHEMA.md`
   and set or clear it in the same edit. Append a line to `data/log.md` under
   today's `## YYYY-MM-DD` group (create the group if it isn't there, newest
   first); if the top-level set of documents changed, update `data/index.md`
   too.
6. **Validate** — `iwe schema validate` must pass. In a git repository, then
   commit with a short message describing the state change (see *Git is
   optional* below).

### What goes where

One line each; `SCHEMA.md` has the fields and the skills have the steps.

- **Post** → `data/drafts/<platform>/YYYY-MM-DD-<slug>.md`, moved to
  `data/posts/<platform>/` when published (the draft skill). On Reddit the
  platform folder nests one level deeper, per subreddit:
  `data/posts/reddit/<subreddit>/`. This is the only nesting rule.
- **Comment in someone else's thread** → thread page, lead, then reply, in the
  order the draft skill gives under *Replying in someone else's thread*; its
  `outcome` at the three-day review (the update skill).
- **Lead converts** → create their `data/people/` record, link it from the lead,
  set the lead's `stage: converted`, move its link in `data/leads.md`.
- **Contact** → the person's doc in `data/people/`, inclusion-linked from its
  role hub.
- **Outreach email** → `data/emails/YYYY-MM-DD-<slug>.md`, recipient linked
  inline; when sent, `stage: sent` with `sent`, and update the person's `stage`.
- **Mention** → `data/mentions/YYYY-MM-DD-<slug>.md`, linked from
  `data/mentions.md` and the venue's `## Mentions`.
- **Interview or survey** → `data/interviews/YYYY-MM-DD-<slug>.md`, interviewee
  linked inline; distill the best language into `data/product.md`.
- **Launch plan step finished** → append a `## Retro` (numbers, what worked)
  before closing it.
- **Venue worked or ruled out** → flip its `stage` (`active`, or `cancelled`
  with the reason in `notes`).
- **Task finished** → `stage: done`, `completed`, and its link moved to
  `## Done` in `data/backlog.md`.
- **Something tried** → `data/experiments/<slug>.md`; on conclusion `verdict`
  and `result`. Before proposing one, check what was tried:
  `iwe find --filter '{type: experiment}' --project '$title,stage,verdict'`.
- **Competitor fact** → `data/competitors/<slug>.md`: dated Changelog entry,
  bump `updated`; `threat` only with the reason in the body.
- **What the record means** → a reading at `data/readings/<slug>.md`, its
  `question` agreed with the user first; linked from `data/readings.md` by
  `impact`, moved to `data/readings-archive.md` when `stage` leaves `open`.
- **Reason to buy** → `data/selling-points/<slug>.md` with its `evidence`. A
  `hypothesis` point never goes into copy as a claim.
- **Outside material** → `data/references/<slug>.md` with the ideas written
  down; `stage: used` and a link where an idea went.
- **Audience segment** → `data/audiences/<slug>.md`; an `inactive` one keeps its
  reason in `notes`.
- **Decision, learning or gotcha** → `data/notes/<slug>.md`. `kind: decision`
  only for what the user stated or confirmed; a replaced note is set
  `superseded`, never deleted.
- **Number** → baselines in `data/metrics.md` (value and dated snapshot); weekly
  numbers as one appended row in `data/metrics-weekly.csv`, never an edit to a
  past row.
- **Positioning or messaging changed** → `data/product.md` and its Changelog,
  then regenerate `.agents/product-marketing.md`.

## Conventions

- **Git is optional.** A workspace created by the iWe app is a plain folder, not
  a git repository; check once per session
  (`git rev-parse --is-inside-work-tree`). Inside a repository, commit each
  validated state change and read `git diff` after a rename or delete. Outside
  one, skip every commit and diff step here and in the skills, and re-read the
  files a rename or delete touched instead. Setup offers `git init` once; do not
  raise it again.
- **Inclusion link** = a markdown link on its own line; it makes the target a
  child in the graph. Hubs inclusion-link their members; links inside sentences
  and list items are soft references. `data/index.md` is link bullets only.
- **Relationships are links, not fields.** A post's venue is the link under the
  venue file's `## Posts`
  (`iwe find --referenced-by data/community-<slug> --filter 'type: post'`). A
  person's role is an inclusion link from a `data/people/role-*.md` hub
  (`iwe find --included-by data/people/role-customer`). A post's pillar is both:
  the `pillar` field to filter on, and an inclusion link from
  `data/pillars/<pillar>.md`, which `data/pillars.md` includes, so the post is
  reachable in the tree.
- **Every publication states `goal` and `success`** — the observable signal that
  means it worked. If you can't name them, the post isn't worth planning.
- **Dated filenames**: posts, mentions, interviews, replies and emails are
  `YYYY-MM-DD-<slug>.md`, kept weekly reports `YYYY-MM-DD.md`; people, leads,
  threads, venues and tasks are undated slugs.
- **In other people's threads the user writes the words.** You find threads,
  read them, and give talking points and accuracy checks; the user writes and
  posts the comment, and the reply's `origin` records who wrote it.
- **Each fact is written once**, on the record made when it happened. A lead
  stores no reply count or "answered" flag — that is read from the replies that
  link to it; a reply stores no thread — the thread page inclusion-links it.
- **Facts and readings stay apart.** A count, a quote or a date is a fact; what
  it means is a reading. Before a reading becomes a rule, write down another
  explanation and the evidence that would tell them apart.
- **Closed items move, they are not deleted** — to the backlog's `## Done`, the
  leads hub's `## Converted` and `## Dropped`, `data/readings-archive.md`.
- **Repurposing trail**: a dated post cut from an evergreen template links back
  to the template inline.
- **Example docs**: `*-example.md` files show each directory's document shape
  with a fictional product, PlainInvoice. Leave them out of any report of real
  campaign state; setup deletes them.
- **Shapes are enforced** by `.iwe/schemas/*.yaml` (`SCHEMA.md` is the readable
  version). Run `iwe schema validate -k <key>` after editing a document. `data/`
  is also an [Open Knowledge
  Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  v0.2 bundle: every document has a non-empty `type`, `data/index.md` stays
  sections of link bullets with no frontmatter beyond `okf_version`, and
  `data/log.md` stays bullets under `## YYYY-MM-DD`. Never unbind a schema to
  get past a failure.
- **Charts read the records**, so never type their numbers into prose. Two
  habits keep them true: every post, mention and reply links its venue page once
  in the body, and every reply's `## Context` opens with the `Person:` line.
  Measurements that aren't documents are CSV or JSON files beside the page that
  charts them, one row per period with ISO dates.
- **Links carry `.md`.** Run `iwe normalize` after bulk edits rather than
  hand-writing link targets.
- **`.agents/product-marketing.md`** is generated by setup from
  `data/product.md`, the brand voice and the current offer, for skill packs that
  read it. Never edit it; edit the sources and regenerate.
- **Web research is part of the job**: backfill, update, thread search and a
  venue refresh fetch public pages. File only what a page you opened shows —
  never invent a number, a quote or a date.

## The `iwe` CLI

- A document's **key** is its path from the workspace root without the extension
  (`data/product`, `data/posts/hn/2026-08-01-launch`).
- **Never `mv` or hand-delete a document** — `iwe rename` and `iwe delete`
  update every link. A deleted document's inclusion links are removed and inline
  links to it become plain text; remove that leftover bullet yourself.
- If `iwe` is not installed, reading and editing still work, but renames,
  queries and validation do not. Ask the user to install it (`index.md` has the
  commands) before restructuring anything.
- Commands and query syntax are in the `iwe` skill
  (`.claude/skills/iwe/SKILL.md`); read it before a query or a restructuring you
  have not run this session.

## Workspace skills

| Skill                              | What it does                                                       |
| ---------------------------------- | ------------------------------------------------------------------ |
| `.claude/skills/setup/SKILL.md`    | Onboarding interview → fills product.md/strategy.md, prunes venues |
| `.claude/skills/backfill/SKILL.md` | Imports existing mentions, posts, competitor facts from the web    |
| `.claude/skills/draft/SKILL.md`    | Drafts a post, or talking points for a thread reply, then files it |
| `.claude/skills/update/SKILL.md`   | Refreshes engagement on published posts and replies                |
| `.claude/skills/weekly/SKILL.md`   | Prints a weekly digest; keeps it in `data/weekly/` when asked      |
| `.claude/skills/iwe/SKILL.md`      | The `iwe` CLI reference: reading, querying, renaming, validating   |
