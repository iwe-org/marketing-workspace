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
  posts, mentions, plan, backlog. Every action you take must leave a record in
  the graph; this is your memory across sessions. Never keep campaign state only
  in conversation.

## Start of every session

1. Read `data/product.md` and `data/strategy.md`. If they are unfilled, run the
   setup flow (`.claude/skills/setup/SKILL.md`) before anything else — marketing
   work without positioning is guessing.
2. Check the backlog and plan:
   `iwe find --filter '{type: task, stage: planned, priority: high}'` and
   `iwe find --filter '{type: plan, stage: in-progress}'`.

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
   - Baseline number learned → `data/metrics.md` (current value + dated
     snapshot).
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
- **Dated filenames**: posts, mentions, and interviews are
  `YYYY-MM-DD-<slug>.md`; entities (people, venues, tasks) are undated slugs.
- **Repurposing trail**: a dated post cut from an evergreen template links back
  to the template inline, so a template's real-world performance stays
  traceable.
- **Example docs**: files suffixed `.example.md` demonstrate each directory's
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
- **Links carry `.md`** — `refs_extension = ".md"`, so a link resolves for
  readers outside iwe. Run `iwe normalize` after bulk edits rather than hand-
  writing link targets.
- **The `.agents/product-marketing.md` bridge**: marketing skill packs read that
  file for product context before asking questions. It is *generated* from
  `data/product.md` (+ brand voice/descriptions, current offer) by the setup
  skill — never edit it directly; edit the source docs and regenerate.

## iwe basics

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

## iwe CLI cheatsheet

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

| Skill                              | What it does                                                             |
| ---------------------------------- | ------------------------------------------------------------------------ |
| `.claude/skills/setup/SKILL.md`    | Onboarding interview → fills product.md/strategy.md, prunes venues       |
| `.claude/skills/backfill/SKILL.md` | Imports existing mentions, posts, competitor facts from the web          |
| `.claude/skills/draft/SKILL.md`    | Drafts a post: positioning + venue rules + pack technique, then files it |
| `.claude/skills/update/SKILL.md`   | Refreshes engagement metrics on published posts                          |
| `.claude/skills/weekly/SKILL.md`   | Prints a weekly digest: engagement, shipped, next priorities             |
