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
iwe tree -d 2                                                    # graph overview: every hub and its members
iwe tree -k data/communities                                     # one hub's subtree
iwe update -k <key> --set stage=published --set 'published="2026-08-01"'
iwe rename data/drafts/hn/x data/posts/hn/x                      # move; references auto-update
iwe delete data/notes/x                                          # delete; inclusion links removed
iwe schema validate                                              # validate all bound docs
```

Filters are YAML (`$eq`, `$ne`, `$in`, `$gte`, `$exists`, …), not jq. On `$key`
there is `$startsWith` but no `$endsWith`, regex or glob: to act on keys by
suffix, list them with `find` and pass each to the command. Structural anchors:
`--includes`, `--included-by`, `--references`, `--referenced-by`, `--roots`.
`--references K` finds the documents that link to K; `--referenced-by K` finds
the documents K links to. `data/index.md` holds link bullets, not inclusion
links, so every hub is a root and `iwe tree -k data/index` prints one line.

Post paths are `data/drafts|posts/<platform>/YYYY-MM-DD-<slug>`, with Reddit
nested per subreddit (`AGENTS.md`, *What goes where*).

## Rules

- Move and delete with `iwe rename` and `iwe delete`, never `mv` or `rm`: they
  update every link in the graph. `iwe delete` removes inclusion links to the
  document and turns inline links to it into plain text, so remove the
  leftover bullet or sentence yourself. Check the result afterwards — with
  `git diff` in a git repository, otherwise by re-reading the files the command
  listed.
- Delete several documents one command each; a `--filter` delete of documents
  that link to each other can fail part-way.
- `iwe schema validate` must pass after every change;
  `iwe schema validate --explain` shows which schema a document is bound to
  when a result is unexpected.
