---
type: hub
description: What you currently think the campaign record means, one reading each, by impact.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Readings

What you currently think the record means, one reading per document
(`data/readings/<slug>.md`). Each keeps the facts, with the queries that
produced them, apart from the reading, names at least one other explanation, and
says what evidence would decide between them. Only `open` readings are listed
here, by `impact`; when one is supported, contradicted or retired, its link
moves to the [archive](readings-archive.md).

``` bash
iwe find --filter '{type: reading, stage: open}' --project '$title,impact,checked'
```

## High impact

## Medium impact

[Replies that leave the product out get answered more often, but replies that
name it start more
trials](readings/answering-without-naming-draws-questions-example.md)

## Low impact

## Archive

[Readings archive](readings-archive.md)
