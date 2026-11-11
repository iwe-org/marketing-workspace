---
type: tracker
description: The campaign's baseline numbers and the dated snapshots that show movement.
stage: living
created: 2026-07-26
stale_after: 2026-10-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Metrics

*The campaign's baseline numbers — the answers to "what's your current
conversion rate / traffic / list size?" that every quantitative skill otherwise
re-asks. Keep the current values in the sections below, and append a dated
snapshot whenever you refresh them, so month-over-month comparison is possible.
Unknown is a valid value — write "unknown" rather than guessing; finding the
number often becomes its own task.*

## Dashboard

Drawn from `data/metrics-weekly.csv`, one row per week (`week`, `visits`,
`signups`, `activated`, `paying_users`, `mrr`). Add a row each week; the charts
update as the file is saved.

``` chart
data: {url: metrics-weekly.csv}
transform:
  - window: [{op: row_number, as: newest}]
    sort: [{field: week, order: descending}]
  - filter: "datum.newest == 1"
  - calculate: "datum.signups / datum.visits"
    as: conversion
hconcat:
  - title: Visits, latest week
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: visits, type: quantitative, format: ","}}
  - title: Signups
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: signups, type: quantitative, format: ","}}
  - title: Visit to signup
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: conversion, type: quantitative, format: ".1%"}}
  - title: Paying users
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: paying_users, type: quantitative}}
  - title: MRR
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: mrr, type: quantitative, format: "$,.0f"}}
```

``` chart
data: {url: metrics-weekly.csv}
hconcat:
  - title: Visits per week
    height: 160
    mark: {type: line, point: true}
    encoding:
      x: &week {field: week, type: temporal, scale: {type: utc}, title: null, axis: {format: "%b %d"}}
      y: {field: visits, type: quantitative, title: null}
      tooltip: [{field: week, type: temporal, scale: {type: utc}, format: "%Y-%m-%d", title: week of}, {field: visits, format: ","}]
  - title: Signups and activated users per week
    height: 160
    transform:
      - fold: [signups, activated]
        as: [step, users]
    mark: {type: line, point: true}
    encoding:
      x: *week
      y: {field: users, type: quantitative, title: null}
      color: {field: step, type: nominal, title: null, legend: {orient: top}, scale: {domain: [signups, activated], range: ["#4a8fd9", "#2e9d5b"]}}
      tooltip: [{field: week, type: temporal, scale: {type: utc}, format: "%Y-%m-%d", title: week of}, {field: step}, {field: users}]
```

## Unit economics

*MRR, churn (voluntary/involuntary split if known), ARPU, CAC per channel, LTV.
For pre-revenue products: write "pre-revenue" and move on.*

✏️

## Funnel

*Visitors → signups → activated → paying: the definition of each step (what
counts as "activated"?) and the current rate between steps. Per key page: its
conversion goal and rate if measured.*

✏️

## Audience

*What you own: email list size, social followings per platform, community
members, RSS/newsletter subscribers. Dated — these feed launch planning and
partner pitches.*

✏️

## Authority

*Domain rating, referring domains, review counts per directory (G2, Capterra,
…), star ratings. Refreshed quarterly is plenty.*

✏️

## Snapshots

*Append-only: a dated subsection per refresh with the values that changed.
Newest first.*
