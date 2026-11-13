---
type: selling-point
description: 'Example selling point: invoice templates live in git, so billing history is diffable and disputes settle themselves.'
kind: differentiator
stage: promoted
pillar: invoice-without-leaving-terminal
strength: high
evidence: customer-reported
created: 2026-07-23
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Invoices version like code

*Example document — this shows the shape of a selling point: one reason to buy,
with the proof you have, the proof still missing, and where the claim is already
used. `## Proof` is the section that keeps unproven claims out of copy. The
setup skill deletes `*-example.md` files after onboarding.*

## The claim

Your invoice templates are plain files in your repo, so every rate change and
every invoice has a history you can diff.

## How it works

`pi init` writes one TOML template per client into the repo. `pi new` reads it
and renders the invoice. Changing a rate is a commit like any other.

## Why it matters

A client disputes an old invoice, and `git log` on the template answers it in
seconds instead of a search through a web app's history.

## Proof

- [Sarah K.](../people/sarah-k-example.md) said it in her own words on HN
  ([mention](../mentions/2026-07-23-sarah-hn-invoicing-thread-example.md)).
- Missing: a customer story of a dispute actually settled this way.

## Where to use it

Developer venues and the [freelance backend
developers](../audiences/freelance-backend-devs-example.md) segment. Already
carried by the [git-native evergreen
template](../evergreen/git-native-workflow-example.md).
