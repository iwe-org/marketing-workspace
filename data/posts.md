---
type: tracker
description: Published posts at a glance — engagement, and whether each met its success line.
stage: living
created: 2026-08-02
generated:
  by: human:author
  at: 2026-08-02T00:00:00Z
---

# Posts

Every publication is a document under `data/posts/<platform>/` once published
and `data/drafts/<platform>/` before. Each is linked from its venue's `## Posts`
and from its pillar hub; this page only charts them. A post's verdict is its
`success` line checked against the evidence (`success_evidence`, `verdict`,
`verdict_date`).

``` bash
iwe find --filter '{type: post, stage: published, verdict: {$exists: false}}' --project '$title,published'   # due a verdict
```

``` chart
datasets:
  published:
    query:
      filter: {type: post, stage: published}
      project: {verdict: 1}
  drafts:
    query:
      filter: {$key: {$startsWith: data/drafts/}}
      project: {stage: 1}
hconcat:
  - title: Published
    data: {name: published}
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {aggregate: count, type: quantitative}}
  - title: In drafts
    data: {name: drafts}
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {aggregate: count, type: quantitative}}
  - title: Met their success line
    data: {name: published}
    height: 50
    transform:
      - filter: "datum.verdict != null && datum.verdict != 'undecidable'"
      - calculate: "datum.verdict == 'met' ? 1 : 0"
        as: met
      - aggregate: [{op: mean, field: met, as: rate}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: rate, type: quantitative, format: ".0%"}}
```

``` chart
data:
  query:
    filter: {type: post, stage: published}
    project: {name: $title, published: 1, platform: 1, goal: 1, verdict: 1, up: engagement.upvotes, cm: engagement.comments}
transform:
  - calculate: "datum.cm || 0"
    as: comments
  - calculate: "datum.up || 0"
    as: upvotes
title: Engagement on published posts
height: 220
mark: {type: circle, opacity: 0.8, stroke: white, strokeWidth: 0.5, cursor: pointer}
encoding:
  x: {field: published, type: temporal, title: null, axis: {format: "%b %d"}}
  y: {field: upvotes, type: quantitative, title: upvotes}
  size: {field: comments, type: quantitative, title: comments, scale: {range: [40, 600]}}
  color: {field: platform, type: nominal, title: null, legend: {orient: top}}
  tooltip: [{field: name, title: post}, {field: platform}, {field: goal}, {field: upvotes}, {field: comments}, {field: verdict}]
```

``` chart
data:
  query:
    filter: {type: post, stage: published}
    project: {goal: 1, verdict: 1, success_evidence: 1}
transform:
  - calculate: "datum.verdict != null ? datum.verdict : datum.success_evidence == 'unobservable' ? 'not observable' : 'not checked yet'"
    as: result
  - calculate: "indexof(['met', 'partly-met', 'not-met', 'undecidable', 'not observable', 'not checked yet'], datum.result)"
    as: rank
title: Did each post meet its success line?
height: 130
mark: bar
encoding:
  y: {field: goal, type: nominal, title: null}
  x: {aggregate: count, type: quantitative, title: posts, axis: {tickMinStep: 1}}
  color:
    field: result
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [met, partly-met, not-met, undecidable, not observable, not checked yet]
      range: ["#2e9d5b", "#e6c34a", "#d9534f", "#a6a6a6", "#c8c8c8", "#e0e0e0"]
  order: {field: rank, aggregate: min, sort: ascending}
  tooltip: [{field: goal}, {field: result, title: verdict}, {aggregate: count, title: posts}]
```
