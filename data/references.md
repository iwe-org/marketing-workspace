---
type: hub
description: Outside material you take marketing ideas from, with the ideas written down.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# References

Outside material you may take ideas from — videos, articles, posts, talks, repos
(`data/references/<slug>.md`). Each records what it shows and the ideas you
could use, and links where an idea went once it is `used`. Content about your
product or your category's argument is [external](mentions.md) instead.

``` bash
iwe find --filter '{type: reference, stage: new}'                       # still to read
iwe find --filter '{type: reference, ideas_for: {$in: [format]}}'       # format ideas
```

[What the best CLI READMEs show in their first
screen](references/readme-demo-teardown-example.md)
