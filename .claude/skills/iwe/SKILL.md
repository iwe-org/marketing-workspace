---
name: iwe
description: The iwe CLI reference for this workspace — filtering and counting documents by frontmatter, reading a document or the graph, updating fields, renaming and deleting without breaking links, and validating. Use before running an iwe query or restructuring documents, or when an iwe command fails.
---

# The iwe CLI

Every command runs from the workspace root. A document's key is its
extension-less path (`data/product`, `data/posts/hn/2026-08-01-launch`).

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

## Rules

- Move and delete with `iwe rename` and `iwe delete`, never `mv` or `rm`: they
  update every link in the graph. Check `git diff` afterwards; the reference
  updates are part of the change.
- `iwe schema validate` must pass before a commit;
  `iwe schema validate --explain` shows which schema a document is bound to
  when a result is unexpected.
