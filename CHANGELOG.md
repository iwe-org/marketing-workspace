# Changelog

All notable changes to this template are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

This is a template you clone, so the entries below double as migration notes: if
you cloned an earlier copy, they tell you what to change in your own workspace.

## [1.2.1]

### Fixed

- The hubs include their example documents. A link such as
  `experiments/annual-plan-discount.example` reads to iwe as a file with the
  extension `.example`, not as a document, so no example was a child of its hub
  and most hubs dropped out of the iWe sidebar. Example documents are now named
  `<name>-example.md`. If you cloned an earlier copy and kept the examples,
  rename them the same way and update the links to them.

## [1.2.0]

### Added

- **Conversations in other people's threads.** Three types that link to each
  other instead of repeating each other: `thread` (`data/threads/`, one page per
  thread where you met people), `reply` (`data/replies/`, one comment you
  posted, with `answers`, `names_product`, `origin`, `engagement` and an
  `outcome` set at a three-day review), and `lead` (`data/leads/`, someone met
  in a thread who may want the product, graduating to `data/people/` on
  conversion). A reply's `## Context` opens with a `Person:` link; the thread
  page inclusion-links its replies.
- **`reading`** (`data/readings/`) — what the campaign record means, with the
  facts and their queries kept apart from the reading, at least one alternative,
  the evidence that would decide it, and an `impact` with its `impact_estimate`.
  Open readings sit on `data/readings.md` by impact; the rest move to the new
  `data/readings-archive.md`.
- **`selling-point`** (`data/selling-points/`) — one reason to buy, with `kind`,
  `strength`, `evidence` and a `## Proof` section naming what is still missing.
  An `evidence: hypothesis` point cannot be marked as in copy.
- **`audience`** (`data/audiences/`), **`email`** (`data/emails/`),
  **`reference`** (`data/references/`, outside material to take ideas from),
  **`note`** (`data/notes/`, short decisions, learnings and gotchas) and
  **`weekly-report`** (`data/weekly/`, a digest kept on request).
- A hub for each new folder, all linked from `data/index.md` under the new
  Conversations and Learning sections, and from the start page.
- Example documents for every new type, continuing the PlainInvoice story: two
  threads and replies (one naming the product, one not), a lead, an open and a
  supported reading, two selling points, two audience segments (one ruled out),
  Devon's pitch and follow-up email, two notes, and a kept week.
- **Table views.** A `.iwe/schemas/<name>.ui.yaml` sidecar for every type you
  browse as a table — 21 in all — with the columns an inclusion row shows,
  titled and colored enum values, and OKF bookkeeping fields hidden from the
  properties band. The validator ignores them.
- **Conditional requirements**, enforced by `iwe schema validate`: a `done` task
  or plan step needs `completed`; a `concluded` experiment needs `verdict`,
  `result` and `concluded`, a `running` one `started`; a `published` post needs
  `published` and `resource`; a reply with an `outcome` needs `reviewed`; a
  dropped lead or inactive audience needs `notes`; a reading's `impact` needs
  `impact_estimate`; outbound email needs `goal` and `success`, sent email its
  `sent` date; a `decision` note needs `origin: human`.
- `post`: `success_evidence`, `verdict`, `verdict_date`, and
  `engagement.shares`.
- `experiment`: `stop_signal`, written before the experiment starts.
- `competitor`: `relation` (`direct | adjacent | incumbent | substitute`),
  optional `threat`, `watch`, a `dormant` stage, and the five body sections are
  now required.
- `community`: `members`, `checked` and `pillar`. `mention`: `pillar`.
- `plan`: the `## Retro` section is declared in the schema.
- **Live charts.** ```` ```chart ```` blocks (Vega-Lite over queries and data
  files) on the replies, leads, mentions, communities, experiments, plan,
  metrics and weekly hubs, and on a new `data/posts.md` page: reply outcomes per
  week, by venue, by author and by whether the product was named; leads answered
  and high-fit leads still waiting; mentions per week by sentiment; activity per
  venue; post engagement and verdicts; experiments by stage; paying users toward
  the first 10; a weekly metrics dashboard of stat tiles and trends.
- `data/metrics-weekly.csv` — weekly visits, signups, activated users, paying
  users and MRR, the data file the metrics and plan charts read. Ships with
  twelve weeks of dummy numbers that setup replaces.
- About eighty more dummy `*.example.md` records from May to early August so
  every chart has a shape: 19 more replies on 11 more threads, 9 more leads, 13
  more mentions, 6 more published posts, 5 more experiments, and a customer who
  started as a lead.

### Changed

- `template.toml` description now names leads, replies and readings; version
  1.2.0.
- The `draft` skill also prepares talking points for a reply in someone else's
  thread and files the thread, lead and reply. The user writes the comment; the
  skill never posts.
- The `update` skill refreshes reply engagement, proposes due outcomes, and
  checks post success lines.
- The `weekly` skill counts replies by outcome, lists replies due review,
  high-fit leads, unanswered email and watched competitors, and writes
  `data/weekly/YYYY-MM-DD.md` only when asked to keep the week.
- The `setup` skill stubs audience segments and selling points from the
  interview; `backfill` files replies the user already posted.
- `AGENTS.md`, `SCHEMA.md`, `STRUCTURE.md` and the README describe the new
  types, the filing order for replies, and the rules behind them: the user
  writes comments in others' threads, each fact is written once, facts stay
  apart from readings, and closed items move to an archive instead of being
  deleted.
- `STRUCTURE.md` no longer carries the template's own publishing and build
  notes; it describes the layout and its rationale only.

### Fixed

- Hub queries and example notes still filtered on `status` after the 1.1.0
  rename (`status: planned`, `status: active`, …) and matched nothing; they use
  `stage` now.
- The `setup` skill counted four workspace skills; there are five.
- The `setup` skill said `iwe delete` removes the hubs' links to example
  documents. It doesn't: a link to an `*.example.md` file is not a graph edge in
  iwe 0.32, so the skill now has you remove those lines with a grep.

### Known issues

- Links to `*.example.md` documents are not graph edges in iwe 0.32 (the
  `.example` reads as a file extension), so example documents do not appear as
  children of their hubs in tree views, and a chart cannot join two example
  records through a link. The charts here avoid such joins. Your own documents,
  without `.example` in the name, are unaffected.

### Migration

If you cloned 1.1.0:

1. Copy the ten new schemas and the `*.ui.yaml` files from `.iwe/schemas/`, and
   the new `[schemas.*]` bindings and `hub` entries from `.iwe/config.toml`.
2. Run `iwe schema validate`. The new conditional rules may flag existing
   documents — most often a done task without `completed`, a concluded
   experiment without `result`, or a competitor profile missing one of its five
   sections. Fill them in rather than loosening the schema.
3. Replace `status:` with `stage:` in any saved `iwe find` filters.

## [1.1.0]

### Added

- `index.md` — a start page at the root: what the workspace is, how to begin,
  and where things live. The iWe app opens it when it creates a workspace from
  this template.
- `preview.png` — the image the iWe template gallery shows for this template.
- `data/` is now a conformant [Open Knowledge
  Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  v0.2 bundle — portable knowledge any OKF consumer can read, not just iwe.
- Three conformance schemas in `.iwe/schemas/` — `okf.yaml` (every document
  carries a non-empty `type`), `okf-index.yaml`, and `okf-log.yaml` (the body
  shapes of the reserved files). They stack on top of the per-type schemas, so a
  document is checked by both.
- OKF's optional families on every type: `description` (one sentence, on all 112
  documents), `generated` (who wrote it and when), `sources` (what it was
  derived from, with per-claim footnotes), `verified`, `resource`,
  `stale_after`, and `tags`.
- `data/log.md` — a date-grouped history of the workspace. The `weekly` skill
  appends to it.
- A GitHub Actions workflow that runs `iwe schema validate` and checks that
  `iwe normalize` is a no-op on every push and pull request.
- `template.toml` — the manifest of the iWe template format (id, name,
  description, version, license), so the template can be listed in the [iWe
  template registry](https://github.com/iwe-org/templates) and created from the
  app with File ▸ New Workspace from Template…. `README.md` and `CHANGELOG.md`
  are excluded from the installed workspace.

### Changed

- **`CLAUDE.md` removed.** It only pointed at `AGENTS.md`, which Claude Code now
  reads directly.
- **`status` is now `stage`.** OKF reserves `status` for its lifecycle values
  (`draft | stable | deprecated`), so the workflow field moved to `stage` in all
  13 schemas and every document. `status` is now set only where it carries
  signal — the mapping is the table in `SCHEMA.md`, and the skills maintain both
  fields together.
- **`plan.stage` is now `growth_stage`.** The growth field
  (`first-10 | first-100 | first-1000`) was renamed to free `stage` for the
  workflow field.
- **`url` is now `resource`** in the six types that had it (community,
  competitor, external, mention, person, post), matching OKF's field for the URI
  of the underlying asset. It is no longer nullable — omit it when unknown.
- **Links carry `.md`.** `refs_extension` changed from `""` to `".md"` so links
  resolve for readers outside iwe. Run `iwe normalize` after changing it.
- **`data/index.md` is the bundle-root index.** Its only frontmatter is
  `okf_version: "0.2"`, and its body is sections of link bullets carrying each
  target's description. It is no longer a `hub` document.
- The `person` and `okf` schema bindings use negated (`!`) glob patterns instead
  of the letter-split globs that worked around globset's missing prefix
  negation.
- **Config version 4.** `.iwe/config.toml` moved from `version = 3` to
  `version = 4`, and the `[library]` table is now `[workspace]`. `iwe normalize`
  performs the migration; it also re-wrapped long link lines in eleven
  documents, with no change to their text.
- **CI runs `iwe` 0.32.0**, up from 0.19.0.
- `.gitignore` ignores `.iwe/app/`, the folder the iWe app keeps its state in.

### Migration

If you cloned an earlier copy of this template, in your workspace:

1. Rename `status:` to `stage:` in every document and schema, then rename
   `plan`'s `stage:` to `growth_stage:`.
2. Rename `url:` to `resource:` in the six types listed above, dropping the key
   where the value was `null`.
3. Set `refs_extension = ".md"` in `.iwe/config.toml` and run `iwe normalize`
   from inside the workspace directory.
4. Copy the three `okf*.yaml` schemas and their `[schemas.okf*]` bindings from
   this template into your `.iwe/`.
5. Add `description` and `generated` to your documents — the agent can backfill
   these in one pass.
6. Run `iwe normalize` with `iwe` 0.32.0 to migrate `.iwe/config.toml` to
   version 4, and add `.iwe/app/` to your `.gitignore`.

Checked with `iwe` 0.32.0, the version CI runs; an older build may not read a
version 4 config. Negated (`!`) schema-binding globs and `iwe init --okf` landed
in 0.19.0.
