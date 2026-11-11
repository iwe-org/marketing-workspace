---
type: hub
description: People met in threads who may want the product, one record each until they graduate to the CRM.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Leads

People spotted in threads who may want the product, one record each
(`data/leads/<handle-slug>.md`). A lead holds only what can't be derived: who
they are, what they need, `fit`, and the `stage` you decided. Everything that
happened is a link — "Where we found them" links the [thread](threads.md), and
every [reply](replies.md) to them opens with a Person link back here — so
whether they answered is read from the replies' `outcome`, never stored twice. A
lead who converts graduates to [People](people.md).

``` bash
iwe find --filter '{type: lead, fit: high, stage: {$in: [new, researched]}}'   # who to approach next
iwe find --referenced-by data/leads/<slug> --filter 'type: reply'               # every reply to one lead
```

## Who answered

Each reply's `Person:` line is matched to a lead by handle, so these tiles count
the leads you replied to and whether any of those replies was answered — read
from the reply records, never stored on the lead.

``` chart
datasets:
  leads:
    query:
      filter: {$key: {$startsWith: data/leads/}}
      project: {handle: 1, stage: 1, fit: 1}
  replies:
    query:
      filter: {$key: {$startsWith: data/replies/}}
      project: {outcome: 1, body: $content}
data: {name: replies}
transform:
  - filter: 'test(/Person: \[/, datum.body)'
  - calculate: 'replace(datum.body, /^[\s\S]*?Person: \[(.+?)\][\s\S]*$/, "$1")'
    as: handle
  - lookup: handle
    from: {data: {name: leads}, key: handle, fields: [stage, fit]}
  - filter: "datum.stage != null"
  - calculate: "indexof(['replied', 'asked', 'will-try', 'tried'], datum.outcome) >= 0 || datum.stage == 'converted' ? 1 : 0"
    as: answered
  - aggregate: [{op: max, field: answered, as: answered}]
    groupby: [handle, stage, fit]
  - calculate: "datum.fit == 'high' && datum.answered == 0 && datum.stage != 'dropped' ? 1 : 0"
    as: waiting
  - aggregate:
      - {op: count, as: contacted}
      - {op: sum, field: answered, as: answered}
      - {op: sum, field: waiting, as: waiting}
  - calculate: "datum.answered / datum.contacted"
    as: rate
hconcat:
  - title: Leads you replied to
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: contacted, type: quantitative}}
  - title: Answered you
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: answered, type: quantitative}}
  - title: Answer rate
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: rate, type: quantitative, format: ".0%"}}
  - title: High fit, still waiting
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: waiting, type: quantitative}}
```

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/leads/}}
    project: {stage: 1, fit: 1}
transform:
  - calculate: "indexof(['converted', 'active', 'researched', 'new', 'dropped'], datum.stage)"
    as: rank
title: Leads by fit and stage
height: 110
mark: bar
encoding:
  y: {field: fit, type: nominal, title: null, sort: [high, medium, low]}
  x: {aggregate: count, type: quantitative, title: leads, axis: {tickMinStep: 1}}
  color:
    field: stage
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [converted, active, researched, new, dropped]
      range: ["#2e9d5b", "#e8a33d", "#4fb3a9", "#4a8fd9", "#a6a6a6"]
  order: {field: rank, aggregate: min, sort: ascending}
  tooltip: [{field: fit}, {field: stage}, {aggregate: count, title: leads}]
```

## Active

[u/hourly_hana](leads/hourly-hana.example)

[u/margin_notes_dev](leads/margin-notes-dev.example)

[u/cobalt_freelance](leads/cobalt-freelance.example)

[u/two_monitor_tess](leads/two-monitor-tess.example)

[bytesized_billing](leads/bytesized-billing.example)

[u/ledger_in_a_jar](leads/ledger-in-a-jar.example)

## Researched

[kettle_studio](leads/kettle-studio.example)

[quillandport](leads/quillandport.example)

## New

[u/homelab_harriet](leads/homelab-harriet.example)

## Converted

[u/tabs_and_totals](leads/tabs-and-totals.example)

## Dropped

[rivetcount](leads/rivetcount.example)
