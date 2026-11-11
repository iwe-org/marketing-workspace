---
type: hub
description: Every comment you posted in someone else's thread, judged by outcome rather than score.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Replies

One record per comment you posted in someone else's thread
(`data/replies/YYYY-MM-DD-<platform>-<slug>.md`); replies under your own posts
stay in the post's body. The measure is `outcome`, set at a three-day review and
raised whenever the conversation moves: `none`, `replied`, `asked`, `will-try`,
`tried`. Score is context. `names_product` and `origin` are the two axes to read
outcomes against: did naming the product help, and who wrote the words.

``` bash
iwe find --filter '{type: reply, outcome: {$exists: false}}' --project '$title,published'   # due a review
iwe find --filter '{type: reply, outcome: {$in: [asked, will-try, tried]}}'                  # replies that started something
```

## What works

Computed from the reply records alone, so nothing here is typed by hand. Bars
are shares of reviewed replies; the number in brackets is how many. The venue is
the venue page each reply links in its `## Context`.

``` chart
datasets:
  replies:
    query:
      filter: {$key: {$startsWith: data/replies/}}
      project: {outcome: 1}
  threads:
    query:
      filter: {$key: {$startsWith: data/threads/}}
      project: {platform: 1}
hconcat:
  - title: Replies
    data: {name: replies}
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {aggregate: count, type: quantitative}}
  - title: Answered
    data: {name: replies}
    height: 50
    transform:
      - filter: "datum.outcome != null"
      - calculate: "datum.outcome == 'none' ? 0 : 1"
        as: answered
      - aggregate: [{op: mean, field: answered, as: rate}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: rate, type: quantitative, format: ".0%"}}
  - title: Led to a trial
    data: {name: replies}
    height: 50
    transform:
      - filter: "datum.outcome != null"
      - calculate: "indexof(['will-try', 'tried'], datum.outcome) >= 0 ? 1 : 0"
        as: trial
      - aggregate: [{op: mean, field: trial, as: rate}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: rate, type: quantitative, format: ".0%"}}
  - title: Waiting for review
    data: {name: replies}
    height: 50
    transform:
      - calculate: "datum.outcome == null ? 1 : 0"
        as: due
      - aggregate: [{op: sum, field: due, as: due}]
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {field: due, type: quantitative}}
  - title: Threads
    data: {name: threads}
    height: 50
    mark: {type: text, fontSize: 30, fontWeight: 600}
    encoding: {text: {aggregate: count, type: quantitative}}
```

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/replies/}}
    project: {outcome: 1, published: 1}
transform:
  - calculate: "datum.outcome == null ? 'not reviewed yet' : datum.outcome == 'none' ? 'no response' : datum.outcome == 'replied' ? 'replied' : datum.outcome == 'asked' ? 'asked about it' : datum.outcome == 'will-try' ? 'will try it' : 'tried it'"
    as: result
  - calculate: "indexof(['tried it', 'will try it', 'asked about it', 'replied', 'no response', 'not reviewed yet'], datum.result)"
    as: rank
title: Replies per week, by outcome
height: 180
mark: bar
encoding:
  x: {field: published, timeUnit: utcyearweek, type: temporal, title: null, axis: {format: "%b %d"}}
  y: {aggregate: count, type: quantitative, title: replies}
  color:
    field: result
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [tried it, will try it, asked about it, replied, no response, not reviewed yet]
      range: ["#1b7f3b", "#2e9d5b", "#8fd19e", "#4a8fd9", "#a6a6a6", "#e0e0e0"]
  order: {field: rank, aggregate: min, sort: ascending}
  tooltip: [{field: published, timeUnit: utcyearweek, title: week of}, {field: result, title: outcome}, {aggregate: count, title: replies}]
```

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/replies/}}
    project: {outcome: 1, refs: $references}
title: Outcome by venue, best answer rate first
transform:
  - filter: "datum.outcome != null"
  - flatten: [refs]
    as: [ref]
  - filter: "indexof(datum.ref.key, 'data/community-') == 0 || indexof(datum.ref.key, 'data/communities/') == 0"
  - calculate: "datum.ref.title"
    as: venue
  - calculate: "datum.outcome == null ? 'not reviewed yet' : datum.outcome == 'none' ? 'no response' : datum.outcome == 'replied' ? 'replied' : datum.outcome == 'asked' ? 'asked about it' : datum.outcome == 'will-try' ? 'will try it' : 'tried it'"
    as: result
  - calculate: "indexof(['tried it', 'will try it', 'asked about it', 'replied', 'no response', 'not reviewed yet'], datum.result)"
    as: rank
  - calculate: "datum.outcome == 'none' ? 0 : 1"
    as: answered
  - joinaggregate:
      - {op: count, as: n}
      - {op: mean, field: answered, as: rate}
    groupby: [venue]
  - calculate: "datum.venue + ' (' + datum.n + ')'"
    as: label
mark: bar
encoding:
  y: {field: label, type: nominal, title: null, sort: {field: rate, op: max, order: descending}}
  x: {aggregate: count, type: quantitative, stack: normalize, title: null, axis: {format: "%"}}
  color:
    field: result
    type: nominal
    title: null
    legend: {orient: top}
    scale:
      domain: [tried it, will try it, asked about it, replied, no response, not reviewed yet]
      range: ["#1b7f3b", "#2e9d5b", "#8fd19e", "#4a8fd9", "#a6a6a6", "#e0e0e0"]
  order: {field: rank, aggregate: min, sort: ascending}
  tooltip: [{field: venue}, {field: result, title: outcome}, {aggregate: count, title: replies}]
```

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/replies/}}
    project: {outcome: 1, origin: 1, names_product: 1}
transform:
  - filter: "datum.outcome != null"
  - calculate: "datum.outcome == null ? 'not reviewed yet' : datum.outcome == 'none' ? 'no response' : datum.outcome == 'replied' ? 'replied' : datum.outcome == 'asked' ? 'asked about it' : datum.outcome == 'will-try' ? 'will try it' : 'tried it'"
    as: result
  - calculate: "indexof(['tried it', 'will try it', 'asked about it', 'replied', 'no response', 'not reviewed yet'], datum.result)"
    as: rank
hconcat:
  - title: Who wrote the words
    transform:
      - calculate: "datum.origin == 'human' ? 'you' : datum.origin == 'agent' ? 'agent draft' : 'agent draft, edited'"
        as: group
      - joinaggregate: [{op: count, as: n}]
        groupby: [group]
      - calculate: "datum.group + ' (' + datum.n + ')'"
        as: label
    mark: bar
    encoding:
      y: {field: label, type: nominal, title: null}
      x: {aggregate: count, type: quantitative, stack: normalize, title: null, axis: {format: "%"}}
      color: &outcome
        field: result
        type: nominal
        title: null
        legend: {orient: top}
        scale:
          domain: [tried it, will try it, asked about it, replied, no response, not reviewed yet]
          range: ["#1b7f3b", "#2e9d5b", "#8fd19e", "#4a8fd9", "#a6a6a6", "#e0e0e0"]
      order: &rank {field: rank, aggregate: min, sort: ascending}
      tooltip: [{field: group, title: written by}, {field: result, title: outcome}, {aggregate: count, title: replies}]
  - title: Names the product
    transform:
      - calculate: "datum.names_product ? 'names it' : 'leaves it out'"
        as: group
      - joinaggregate: [{op: count, as: n}]
        groupby: [group]
      - calculate: "datum.group + ' (' + datum.n + ')'"
        as: label
    mark: bar
    encoding:
      y: {field: label, type: nominal, title: null}
      x: {aggregate: count, type: quantitative, stack: normalize, title: null, axis: {format: "%"}}
      color: *outcome
      order: *rank
      tooltip: [{field: group}, {field: result, title: outcome}, {aggregate: count, title: replies}]
```

## All replies, newest first

[Replying to quillandport about an export
format](replies/2026-08-02-hn-replying-to-quillandport-about-an.example)

[Answering u/two_monitor_tess on payment
links](replies/2026-08-01-reddit-answering-u-two_monitor_tess-on-payment.example)

[Adding the per-client pricing argument to the
thread](replies/2026-07-30-indiehackers-adding-the-per-client-pricing-argument.example)

[Answering bytesized_billing on pricing that suits a slow
month](replies/2026-07-29-indiehackers-answering-bytesized_billing-on-pricing-that.example)

[Answering u/hourly_hana on keeping invoice templates in the
repo](replies/2026-07-28-reddit-webdev-invoicing-without-saas.example)

[Thanking Sarah for the recommendation in the Ask HN invoicing
thread](replies/2026-07-23-hn-ask-hn-invoicing.example)

[Answering u/two_monitor_tess on CLI-only
products](replies/2026-07-15-reddit-answering-u-two_monitor_tess-on-cli-only.example)

[Answering u/ledger_in_a_jar on keeping invoices with the
contracts](replies/2026-07-09-reddit-answering-u-ledger_in_a_jar-on-keeping.example)

[Adding invoices to the list of non-code things in
git](replies/2026-07-08-reddit-adding-invoices-to-the-list.example)

[Telling u/cobalt_freelance which tool it
was](replies/2026-07-03-reddit-telling-u-cobalt_freelance-which-tool.example)

[Answering u/cobalt_freelance on whether a billing tool pays for
itself](replies/2026-07-01-reddit-answering-u-cobalt_freelance-on-whether.example)

[Asking quillandport how invoices fit a plain-text
ledger](replies/2026-06-24-hn-asking-quillandport-how-invoices-fit.example)

[Sharing what changed after week four of a CLI
tool](replies/2026-06-17-reddit-sharing-what-changed-after-week.example)

[Answering the offline invoicing question on
r/selfhosted](replies/2026-06-10-reddit-answering-the-offline-invoicing-question.example)

[Answering kettle_studio on invoicing for two
founders](replies/2026-06-03-indiehackers-answering-kettle_studio-on-invoicing-for.example)

[Answering u/margin_notes_dev again, without the product
name](replies/2026-05-28-reddit-answering-u-margin_notes_dev-again-without.example)

[Pointing u/margin_notes_dev at PlainInvoice for invoice
history](replies/2026-05-27-reddit-pointing-u-margin_notes_dev-at-plaininvoice.example)

[Following up with u/tabs_and_totals after their first
invoice](replies/2026-05-20-reddit-following-up-with-u-tabs_and_totals.example)

[Answering rivetcount on one tool for a whole
agency](replies/2026-05-19-hn-answering-rivetcount-on-one-tool.example)

[Listing PlainInvoice in the tools-you-pay-for
roundup](replies/2026-05-13-reddit-listing-plaininvoice-in-the-tools-you-pay-for.example)

[Answering u/tabs_and_totals on billing five clients from a
spreadsheet](replies/2026-05-12-reddit-answering-u-tabs_and_totals-on-billing.example)
