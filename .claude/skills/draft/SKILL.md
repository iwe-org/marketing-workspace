---
name: draft
description: Draft a post for a specific venue — combines positioning from data/product.md, the venue's posting rules, and the installed marketing skill pack's technique, then files the draft into the pipeline with full frontmatter. Use when the user says "draft a post for <venue>", "write the Product Hunt launch", or similar.
---

# Draft a post

A draft is only useful if it fits the venue's rules, carries the product's
positioning, and states why it exists. This skill produces the draft *and* the
record.

## Steps

1. **Preconditions.** `data/product.md` must be filled (no ✏️ blocks) —
   otherwise run the setup skill first. Identify the target venue entity
   (`data/community-<slug>.md`, or `data/communities/reddit/<name>.md` for
   subreddits); read its `notes` (posting rules, flair, timing) and `platform`.
   If the venue's rules are marked unverified, tell the user to check them
   before publishing.
2. **Intent.** Establish with the user (or propose): `goal` (awareness |
   conversion | positioning | credibility | retention), `success` (one
   observable signal), and which messaging `pillar` this post carries. A post
   whose goal you can't name isn't worth drafting.
3. **Technique.** If the installed marketing skill pack has a matching skill
   (copywriting, launch strategy, the venue's channel), apply it for the copy.
   Baseline rules regardless: open with what the reader can do or learn —
   never the origin story; match the venue's native tone; on community venues,
   write as a member sharing work, not a marketer placing content.
4. **File it.** Write `data/drafts/<platform>/YYYY-MM-DD-<slug>.md`:

   ``` yaml
   ---
   type: post
   pillar: <slug or null>
   goal: <goal>
   success: "<one line>"
   status: draft
   platform: <platform>
   created: <today>
   published: null
   url: null
   ---
   # <Post title>

   <post body, ready to copy-paste to the venue>
   ```

5. **Link the graph.** Add a link to the draft under the venue file's
   `## Posts` heading (create the heading if it doesn't exist yet), and an
   inclusion link from the pillar's hub (`data/pillars/<pillar>.md`) so the
   post stays reachable in the tree.
6. **Validate & commit.** `iwe schema validate` must pass; commit
   `draft: <venue> — <title>`.
7. **On publish** (when the user reports it's live): set `url`, `published`,
   `status: published`, and move the doc:
   `iwe rename data/drafts/<platform>/<file> data/posts/<platform>/<file>`.

## Rules

- One venue per draft — venues have different rules; a cross-posted text is a
  smell.
- Titles do most of the work; offer the user 2–3 title options before
  finalizing.
- Never fabricate proof — pull claims only from `data/product.md`'s Proof
  section.
