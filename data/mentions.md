---
type: hub
description: Every third-party mention of the product — comments, threads, listings, reviews.
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# Mentions

Every third-party mention of your product — comments, threads, listings, reviews
— one dated document each (`data/mentions/YYYY-MM-DD-<slug>.md`), plus
article-length pieces filed as `data/external-*.md`. Mentions are leads: a
positive mention is outreach warmed up for you; file the author in the CRM and
follow up.

``` chart
data:
  query:
    filter: {$key: {$startsWith: data/mentions/}}
    project: {published: 1, sentiment: 1, platform: 1}
transform:
  - calculate: "indexof(['positive', 'mixed', 'neutral', 'negative'], datum.sentiment)"
    as: rank
hconcat:
  - title: Mentions per week, by sentiment
    height: 160
    mark: bar
    encoding:
      x: {field: published, timeUnit: utcyearweek, type: temporal, title: null, axis: {format: "%b %d"}}
      y: {aggregate: count, type: quantitative, title: mentions}
      color: &sentiment
        field: sentiment
        type: nominal
        title: null
        legend: {orient: top}
        scale:
          domain: [positive, mixed, neutral, negative]
          range: ["#2e9d5b", "#e6c34a", "#a6a6a6", "#d9534f"]
      order: &rank {field: rank, aggregate: min, sort: ascending}
      tooltip: [{field: published, timeUnit: utcyearweek, title: week of}, {field: sentiment}, {aggregate: count, title: mentions}]
  - title: Where
    height: 160
    mark: bar
    encoding:
      y: {field: platform, type: nominal, title: null, sort: -x}
      x: {aggregate: count, type: quantitative, title: mentions}
      color: *sentiment
      order: *rank
      tooltip: [{field: platform}, {field: sentiment}, {aggregate: count, title: mentions}]
```

Atomic mentions:

[maple_and_mortar on PlainInvoice,
2026-07-30](mentions/2026-07-30-maple-and-mortar-producthunt-review.example)

[Sarah recommends PlainInvoice in an HN invoicing-tools
thread](mentions/2026-07-23-sarah-hn-invoicing-thread.example)

[u/the_other_tab on PlainInvoice,
2026-07-22](mentions/2026-07-22-the-other-tab-sideproject-currency.example)

[u/freelance_fern on PlainInvoice,
2026-07-21](mentions/2026-07-21-freelance-fern-sideproject-launch.example)

[u/shipping_sundays on PlainInvoice,
2026-07-18](mentions/2026-07-18-shipping-sundays-buildinpublic.example)

[@latte_lambda on PlainInvoice,
2026-07-11](mentions/2026-07-11-latte-lambda-x-invoice-day.example)

[pinecone_parser on PlainInvoice,
2026-07-04](mentions/2026-07-04-pinecone-parser-hn-diffable.example)

[u/rack_and_roll_rhea on PlainInvoice,
2026-06-26](mentions/2026-06-26-rack-and-roll-selfhosted-payment-api.example)

[Terminal Craft on PlainInvoice,
2026-06-19](mentions/2026-06-19-terminal-craft-newsletter-listing.example)

[u/semicolon_sally on PlainInvoice,
2026-06-12](mentions/2026-06-12-semicolon-sally-webdev-spreadsheet.example)

[tidepool_studio on PlainInvoice,
2026-06-06](mentions/2026-06-06-tidepool-studio-ih-bookkeeper.example)

[@quietcommit_dev on PlainInvoice,
2026-05-30](mentions/2026-05-30-quietcommit-x-between-reviews.example)

[fennel_and_fig on PlainInvoice,
2026-05-22](mentions/2026-05-22-fennel-and-fig-hn-cli-billing.example)

[u/ledgerless_lou on PlainInvoice,
2026-05-15](mentions/2026-05-15-ledgerless-lou-sideproject-two-clients.example)

Articles:

[Terminal freelance stack roundup](external-terminal-invoicing-roundup.example)

## Watchlist

*The input side of this page — what you listen for. One line per term:
`keyword/phrase — where — alert tool`. Cover the product name (and
misspellings), your category phrases, competitor names, and the bottom-of-funnel
problem phrases people type when they hurt ("invoicing without leaving the
terminal", not just "invoicing"). Free alert tools exist for Hacker News and
Reddit keyword tracking. Review the list whenever positioning changes.*

✏️
