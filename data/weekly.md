---
type: hub
description: Weekly digests you chose to keep on file, newest first.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Weekly reports

The weekly digests you asked to keep (`data/weekly/YYYY-MM-DD.md`), newest
first. The weekly skill prints a digest every run and writes one here only on
request; `data/log.md` holds the line-by-line history either way.

``` chart
data:
  query:
    filter:
      $or:
        - {$key: {$startsWith: data/posts/}}
        - {$key: {$startsWith: data/replies/}}
        - {$key: {$startsWith: data/mentions/}}
        - {$key: {$startsWith: data/emails/}}
    project: {type: 1, published: 1, sent: 1}
transform:
  - calculate: "datum.type == 'email' ? datum.sent : datum.published"
    as: day
  - filter: "datum.day != null"
  - calculate: "toDate(datum.day)"
    as: date
  - calculate: "{'post': 'posts published', 'reply': 'replies', 'mention': 'mentions', 'email': 'emails sent'}[datum.type]"
    as: kind
title: Activity per week
height: 180
mark: bar
encoding:
  x: {field: date, timeUnit: utcyearweek, type: temporal, title: null, axis: {format: "%b %d"}}
  y: {aggregate: count, type: quantitative, title: items}
  color:
    field: kind
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [posts published, replies, mentions, emails sent]
      range: ["#2f7de1", "#2e9d5b", "#e8a33d", "#8e6bbf"]
  tooltip: [{field: date, timeUnit: utcyearweek, title: week of}, {field: kind}, {aggregate: count, title: items}]
```

[Week of 2026-07-20](weekly/2026-07-27-example.md)
