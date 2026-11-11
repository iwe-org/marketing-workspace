---
type: hub
description: The audience segments you market to — what lands with each and where they gather.
stage: living
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Audiences

One document per audience segment (`data/audiences/<slug>.md`): who they are,
what resonates, what to avoid, the pitches that work for them, and the venues
where they gather. The personas in [Product](product.md) say who the product is
for; a segment here is the unit you plan venues and copy around. Segments you
ruled out stay, as `inactive`, with the reason.

``` bash
iwe find --filter '{type: audience, stage: active}' --project '$title,fit,pillar'
```

[Freelance backend developers](audiences/freelance-backend-devs-example.md)

[Small agencies](audiences/small-agencies-example.md)
