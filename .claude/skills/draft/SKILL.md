---
name: draft
description: Draft a post for a specific venue — combines positioning from data/product.md, the venue's posting rules, and the installed marketing skill pack's technique, then files the draft into the pipeline with full frontmatter. Also prepares talking points for a reply in someone else's thread and files the thread, lead and reply records. Use when the user says "draft a post for <venue>", "write the Product Hunt launch", "find threads worth answering", "help me reply to this thread", or similar.
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
   whose goal you can't name isn't worth drafting. Pick the argument from
   `data/selling-points/` (`stage: validated` or `promoted`) before inventing
   one, and check `data/audiences/` for what to avoid with this crowd.
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
   stage: draft
   platform: <platform>
   created: <today>
   published: null
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
7. **On publish** (when the user reports it's live): set `resource`, `published`,
   `stage: published`, and move the doc:
   `iwe rename data/drafts/<platform>/<file> data/posts/<platform>/<file>`.

## Rules

- One venue per draft — venues have different rules; a cross-posted text is a
  smell.
- Titles do most of the work; offer the user 2–3 title options before
  finalizing.
- Never fabricate proof — pull claims only from `data/product.md`'s Proof
  section or a selling point whose `evidence` is not `hypothesis`.

## Replying in someone else's thread

The user writes the words of every comment posted in other people's threads;
this mode prepares and records, it never posts.

1. **Find.** When asked to find threads, search the venues in
   `data/communities.md` the user named for the problem phrases in
   `data/product.md` (Customer language) and the watchlist in
   `data/mentions.md`, not the product name. Skip threads already filed
   (`iwe find --filter '{type: thread}' --project '$title,resource'`).
   Present a shortlist: link, age, what was asked, and why it fits.
2. **Read.** For the thread the user picks, read the whole thread and the
   venue's `notes`. Say whether the venue allows naming the product here.
3. **Talking points.** Give two to four talking points that answer what was
   literally asked, the facts to get right, and anything to avoid. No finished
   comment unless the user asks for one; if they do, mark it as a draft for
   them to rewrite.
4. **File** once the user says it is posted, in this order: the thread page
   `data/threads/<platform>-<slug>.md` (link the venue in its body); a lead
   `data/leads/<handle-slug>.md` (`stage: new`, `fit`, `## Where we found
   them`, `## What they need`) if the person answered may want the product and
   has no lead or person record yet; then the reply
   `data/replies/YYYY-MM-DD-<platform>-<slug>.md` with `## Context` opening
   `Person: [<handle>](<lead or person>)`, `## Our reply` quoting the comment
   exactly as posted, `names_product`, and `origin` (`human` when the user
   wrote it, `agent-edited` or `agent` only when they say so). Inclusion-link
   the reply under the thread page's `## Replies` and from `data/replies.md`;
   link the lead from `data/leads.md`.
5. **Validate & commit.** `iwe schema validate`; commit
   `reply: <venue> — <thread title>`.
