---
name: setup
description: Guided workspace onboarding — interview the founder, fill in data/product.md and the companion trackers (strategy, brand, offer, metrics), prune the seeded venue list to the ICP, activate the matching stage plan, and export the .agents/product-marketing.md bridge file that marketing skill packs read. Use when the user says "run the setup", "set up the workspace", or when product.md still contains ✏️ placeholders.
---

# Workspace setup

Turn the blank workspace into this founder's campaign memory. Everything
downstream (drafts, venue choices, outreach, every skill-pack skill) reads what
you write here — optimize for the founder's own language, not marketing-speak.

## Steps

1. **Assess.** Read `data/product.md`, `data/strategy.md`, `data/brand.md`,
   `data/offer.md`, `data/metrics.md`. List which ✏️ blocks are unfilled. If
   everything is filled, ask what to revise instead of re-interviewing.
   Also check whether a marketing skill pack is installed (technique skills
   present in `.claude/skills/` or `.agents/skills/` beyond this workspace's
   four). If not, offer — once — to install the recommended one:
   `npx skills add coreyhaines31/marketingskills -a claude-code`
   (the `-a` flag matters when running from inside an agent session). If
   declined, offline, or it fails: continue without comment — the pack is
   optional, and the workspace works fully without it. Note what's installed
   (pack name + version if visible) in a one-line "Toolkit" note at the bottom
   of `data/strategy.md`.
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
   `data/pillars/<slug>.md` (`type: hub`, `stage: living`, a one-line
   statement of the messaging claim) and inclusion-link each from the Pillars
   section of `data/index.md` — every future publication gets an inclusion
   link from its pillar hub, which is what keeps posts reachable in the tree.
4. **Create competitor stubs.** For each competitor named in the interview,
   create `data/competitors/<slug>.md` (stage: active, resource, one-line stance
   in `notes`) and link it from the competitors hub; deep profiling stays a
   backlog task.
5. **Prune venues.** Walk
   `iwe find --filter '{type: community, stage: planned}' -f json`
   against the ICP. Propose a shortlist (~10 to keep) and the cancellations
   with one-line reasons; on confirmation set `stage: cancelled` and append
   the reason to `notes` for each cut venue
   (`iwe update -k <venue key> --set stage=cancelled` — keys are
   `data/community-<slug>` for standalone sites,
   `data/communities/reddit/<name>` for subreddits).
6. **Activate the plan.** In `data/plan.md`, put the matching stage plan under
   `## Now` (others under Next/Later in stage order) and set its frontmatter
   `stage: in-progress`.
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
   move their links in `data/backlog.md` to the `## Done` section. Delete the
   example docs — every `*.example.md` under `data/` — now that real documents
   replace their job. `iwe delete` removes their inclusion links from hubs
   automatically but *flattens* inline links to plain text: sweep the venue
   files' `## Posts`/`## Mentions` sections and remove the leftover de-linked
   example lines.
9. **Validate & commit.** `iwe schema validate` must pass. Commit with a
   message like
   `setup: product/strategy/brand/offer filled, venues pruned to <n>, <stage> plan activated`.

## Rules

- Never leave a ✏️ block half-filled — ask follow-ups until each section is
  concrete, but let the user say "skip for now" (leave the ✏️ so the gap stays
  visible).
- Pillars are slugs (`lowercase-hyphenated`), each with a one-line description.
- Don't cancel venues silently; the user confirms the pruning list first.
- Never invent numbers, quotes, or competitor facts — "unknown" beats plausible
  fiction.
