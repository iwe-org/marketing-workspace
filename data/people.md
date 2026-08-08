---
type: hub
description: The campaign's CRM — one document per person, grouped by the role they play.
stage: living
created: 2026-07-26
generated:
  by: human:author
  at: 2026-07-26T00:00:00Z
---

# People

The campaign's CRM. One document per person (`data/people/<slug>.md`); their
roles are inclusion links from the role hubs below — a person can hold several.
Track outreach state in the person's `status` field
(`planned → contacted → replied/declined`).

``` bash
iwe find --included-by data/people/role-prospect --filter 'status: contacted'
```

[Customers](people/role-customer.md)

[Prospects](people/role-prospect.md)

[Amplifiers](people/role-amplifier.md)

[Creators](people/role-creator.md)

[Contacts](people/role-contact.md)
