---
type: tracker
description: Campaign-level moves, each a plan step with its own definition of done.
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Plan

Campaign-level moves, each a document in `data/plan/` with a
`## Definition of done`. Seeded with one plan per growth stage — activate the
one matching your current stage (`data/strategy.md`) and leave the rest
`planned`. A plan step's definition of done may inclusion-link the backlog tasks
that fulfil it, so task status rolls up.

Significant launches (Product Hunt, Show HN, a major release) get their own plan
step — a launch is prep, day-of, and follow-up, not a single post. When a launch
step completes, append a `## Retro` section to it: the numbers, what worked,
what to skip next time. You will launch again; the next one should start from
evidence, not memory.

Progress toward the current milestone, from the weekly numbers in
`data/metrics-weekly.csv` and the plan steps' `stage`:

``` chart
datasets:
  weeks: {url: metrics-weekly.csv}
  steps:
    query:
      filter: {type: plan}
      project: {stage: 1}
hconcat:
  - title: Paying users
    data: {name: weeks}
    height: 50
    transform:
      - window: [{op: row_number, as: newest}]
        sort: [{field: week, order: descending}]
      - filter: "datum.newest == 1"
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: paying_users, type: quantitative}}
  - title: To the first 10
    data: {name: weeks}
    height: 50
    transform:
      - window: [{op: row_number, as: newest}]
        sort: [{field: week, order: descending}]
      - filter: "datum.newest == 1"
      - calculate: "max(10 - datum.paying_users, 0)"
        as: left
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: left, type: quantitative}}
  - title: Plan steps in progress
    data: {name: steps}
    height: 50
    transform:
      - calculate: "datum.stage == 'in-progress' ? 1 : 0"
        as: active
      - aggregate: [{op: sum, field: active, as: active}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: active, type: quantitative}}
  - title: Plan steps done
    data: {name: steps}
    height: 50
    transform:
      - calculate: "datum.stage == 'done' ? 1 : 0"
        as: done
      - aggregate: [{op: sum, field: done, as: done}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: done, type: quantitative}}
```

``` chart
data: {url: metrics-weekly.csv}
title: Paying users toward the first 10
height: 160
encoding:
  x: {field: week, type: temporal, scale: {type: utc}, title: null, axis: {format: "%b %d"}}
layer:
  - mark: {type: line, point: true}
    encoding:
      y: {field: paying_users, type: quantitative, title: paying users, scale: {domain: [0, 10]}}
      tooltip: [{field: week, type: temporal, scale: {type: utc}, format: "%Y-%m-%d", title: week of}, {field: paying_users, title: paying users}]
  - mark: {type: rule, strokeDash: [4, 4], color: "#a6a6a6"}
    encoding:
      y: {datum: 10}
```

## Now

[Get the first 10 users](plan/first-10-users.md)

## Next

[Get the first 100 users](plan/first-100-users.md)

## Later

[Get the first 1000 users](plan/first-1000-users.md)

## Done

[Launch on r/SideProject](plan/sideproject-launch-example.md)
