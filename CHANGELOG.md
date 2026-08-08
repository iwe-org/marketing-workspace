# Changelog

All notable changes to this template are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

This is a template you clone, so the entries below double as migration notes: if
you cloned an earlier copy, they tell you what to change in your own workspace.

## [Unreleased]

### Added

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

### Changed

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

Requires `iwe` 0.19.0 or newer — negated (`!`) schema-binding globs and
`iwe init --okf` landed there.
