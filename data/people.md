---
type: hub
status: living
created: 2026-07-26
---

# People

The campaign's CRM. One document per person (`data/people/<slug>.md`); their
roles are inclusion links from the role hubs below — a person can hold several.
Track outreach state in the person's `status` field
(`planned → contacted → replied/declined`).

``` bash
iwe find --included-by data/people/role-prospect --filter 'status: contacted'
```

[Customers](people/role-customer)

[Prospects](people/role-prospect)

[Amplifiers](people/role-amplifier)

[Creators](people/role-creator)

[Contacts](people/role-contact)
