---
name: setup
description: Guided workspace onboarding — interview the founder, fill in data/product.md and the companion trackers (strategy, brand, offer, metrics), prune the seeded venue list to the ICP and re-read the kept venues' posting rules on the web, activate the matching stage plan, delete the example documents, offer git init once, and export the .agents/product-marketing.md bridge file that marketing skill packs read. Use when the user says "run the setup" or "set up the workspace", or when What is it, Ideal customer profile, Positioning or Messaging pillars in data/product.md still shows a bare ✏️ (one marked "✏️ skipped" does not count).
---

# Workspace setup

Turn the blank workspace into this founder's campaign memory. Everything
downstream (drafts, venue choices, outreach) reads what you write here —
optimize for the founder's own language, not marketing-speak.

## Steps

1. **Assess.** Read `data/product.md`, `data/strategy.md`, `data/brand.md`,
   `data/offer.md`, `data/metrics.md`. List which ✏️ blocks are unfilled; a
   block marked `✏️ skipped <date>` was left on purpose — ask about it once,
   and leave it if the answer is still "later". If everything is filled, ask
   what to revise instead of re-interviewing. Two one-time notes, neither of
   which you repeat or act on unasked:
   - *Git.* If `git rev-parse --is-inside-work-tree` fails, the folder is not a
     repository (the iWe app creates a plain folder). Offer `git init` once, so
     the campaign has a history; run it only on a yes. Without git, skip the
     commit in the last step.
   - *Skill pack.* Mention that a marketing skill pack such as
     [marketingskills](https://github.com/coreyhaines31/marketingskills) can
     add copywriting and launch technique, and that the user installs it
     themselves from its own instructions. Never run an installer. If technique
     skills beyond this workspace's six are already present, record the pack in
     a one-line "Toolkit" note at the bottom of `data/strategy.md`.
2. **Interview.** Ask in batches, conversationally (don't interrogate all at
   once):
   - *Product*: the party-sentence one-liner; who exactly it's for and where
     they hang out; who it's NOT for (disqualifiers, anti-persona).
   - *Alternatives*: what people use today; the sharpest difference; the 2–4
     competitors worth tracking.
   - *Evidence*: proof points; objections heard so far and the honest
     responses; verbatim phrases customers use for the problem.
   - *Money*: current tiers/pricing, the free-vs-paid boundary, guarantee.
   - *Voice*: how it should sound, words to ban; existing brand assets and
     where they live.
   - *Reality*: honest stage (first-10 / first-100 / first-1000); weekly time
     budget; platforms they're comfortable on; baseline numbers they know
     offhand.
3. **Write the docs.** Fill `data/product.md` (all sections, keeping the
   founder's vivid phrasing; derive 3–5 pillar slugs and confirm them),
   `data/strategy.md` (stage, 2–3 channels justified against the ICP,
   observable goals, cadence), `data/brand.md` (voice + description variants —
   draft the 60-char and 150-word variants from the one-liner and confirm),
   `data/offer.md`, and `data/metrics.md` (what's known; "unknown" is a valid
   value). Delete the italic instruction lines as sections fill. Add a dated
   entry to product.md's Changelog. Create a hub per confirmed pillar at
   `data/pillars/<slug>.md` (`type: hub`, `stage: living`, `created`, a
   one-line statement of the messaging claim) and inclusion-link each from
   `data/pillars.md` — a link on its own line. Every future publication gets
   an inclusion link from its pillar hub, which keeps posts reachable in the
   tree. `data/index.md` already lists `data/pillars.md`; it takes link
   bullets only, never an inclusion link.
4. **Create competitor stubs and segments.** For each competitor named in the
   interview, create `data/competitors/<slug>.md` (stage: active, resource,
   `relation`, one-line stance in `notes`, and the five body sections with ✏️
   where unknown) and link it from the competitors hub; deep profiling stays a
   backlog task. For each audience segment the founder named, create
   `data/audiences/<slug>.md` (`stage: active` or `planned`) with the five
   sections, and link it from `data/audiences.md`. Each proof point the
   founder can show becomes a `data/selling-points/<slug>.md` stub with its
   `evidence`.
5. **Prune venues.** The seeded list is a menu, not a plan: nothing on it is
   worked until the founder picks it.
   - *Deal platforms first.* The fourteen `category: deals` venues sell
     lifetime access at a steep one-time discount. Ask one question: is a
     lifetime deal something this product would ever run? Unless the answer
     is yes, cancel the whole category in one command and do not raise it
     again:

     ``` bash
     iwe update --filter '{type: community, category: deals}' \
       --set stage=cancelled --set status=deprecated --expect 14
     ```

     The venues' `notes` keep their posting rules; record the reason once, as
     a decision note in `data/notes/` (`kind: decision`, `origin: human`)
     linked from `data/notes.md`.
   - *Then the rest.* Walk
     `iwe find --filter '{type: community, stage: planned}' -f json`
     against the ICP. Propose a shortlist (~10 to keep) and the
     cancellations with one-line reasons. On confirmation, for each cut venue
     append the reason to `notes` and run
     `iwe update -k <venue key> --set stage=cancelled --set status=deprecated`
     — a cancelled venue is `status: deprecated` (the table in `SCHEMA.md`).
     Keys are `data/community-<slug>` for standalone sites and
     `data/communities/reddit/<name>` for subreddits.
   - *Refresh what stays.* The seeds were researched on their `created`
     date, and some say "rules unverified" in `notes`. For each venue kept
     whose `stale_after` has passed or falls within the next month, or whose
     rules are unverified, open its `resource`, correct the posting rules in
     `notes` and the body, and set `stale_after` six months out with a
     `verified` stamp. If the page cannot be reached, leave the date and say
     so in `notes`. Cancelled venues are not refreshed.
6. **Activate the plan.** In `data/plan.md`, put the matching stage plan under
   `## Now` (others under Next/Later in stage order) and set its frontmatter
   `stage: in-progress`. The two charts on that page count toward a target of
   10; for `first-100` or `first-1000`, change the target in the three places
   it is written: `max(10 - datum.paying_users, 0)`, `domain: [0, 10]` and
   `y: {datum: 10}`.
7. **Export the bridge file.** Marketing skill packs (e.g. marketingskills)
   read `.agents/product-marketing.md` before asking questions. Generate it:
   start with the line
   `<!-- Generated from data/product.md — edit there and re-run setup to refresh. -->`,
   then the filled content of product.md (all sections including Changelog),
   the Voice and Descriptions sections from brand.md, and the Current offer
   section from offer.md. Regenerate this file whenever those sections
   materially change.
8. **Close the loop.** Mark the onboarding tasks done (`fill-product-doc`,
   `set-strategy`, `prune-venues`): `stage: done`, `completed: <today>`, and
   move their links in `data/backlog.md` to the `## Done` section. Then remove
   the example documents — every `*-example.md` under `data/` — in this order,
   from the workspace root:

   1. Drop the list bullets that link to an example, while the links are
      still there to find (the venue files' `## Posts` and `## Mentions`):

      ``` bash
      grep -rlE -- '-example\.md\)' data | grep -v -- '-example\.md$' \
        | xargs perl -0pi -e 's/^- (?:(?!\n- |\n\n|\n#).)*?-example\.md\)(?:(?!\n- |\n\n|\n#).)*\n//gms'
      ```

   2. Delete the examples one at a time; `iwe delete` removes the hubs'
      inclusion links to each. A single `--filter` delete of all of them fails
      part-way, so use the loop:

      ``` bash
      find data -name '*-example.md' | sed 's/\.md$//' | sort \
        | while read -r key; do iwe delete "$key" --quiet || echo "FAILED $key"; done
      ```

   3. Tidy the files that changed, then check that nothing is left. The two
      checks must print nothing:

      ``` bash
      iwe normalize
      find data -name '*-example.md'
      grep -rniE 'plaininvoice|-example' data
      ```

      If a line is left that is not the founder's own text, open the file
      and remove the bullet or sentence by hand.

   Then reset `data/metrics-weekly.csv` to its header row
   (`week,visits,signups,activated,paying_users,mrr`) plus the founder's real
   weeks, if they have any — its current rows are dummy numbers.
9. **Validate.** `iwe schema validate` must pass. In a git repository, commit
   with a message like
   `setup: product/strategy/brand/offer filled, venues pruned to <n>, <stage> plan activated`.

## Rules

- Never leave a ✏️ block half-filled — ask follow-ups until each section is
  concrete. When the user says "skip for now", replace the block with
  `✏️ skipped <today's date>` so the gap stays visible. A skipped section does
  not trigger setup again and does not block drafting; mention it when the
  work in front of you needs it.
- Pillars are slugs (`lowercase-hyphenated`), each with a one-line description.
- Don't cancel venues silently; the user confirms the pruning list first.
- Never invent numbers, quotes, or competitor facts — "unknown" beats plausible
  fiction.
