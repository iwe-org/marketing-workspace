---
type: email
description: 'Example scheduled follow-up: one short nudge to the newsletter writer, ten days after the pitch.'
stage: scheduled
status: draft
direction: outbound
target: Devon (Tools Weekly)
subject: 'Re: A terminal invoicing tool for your terminal corner'
pillar: invoice-without-leaving-terminal
goal: awareness
success: Devon replies either way.
sent: null
created: 2026-08-01
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Follow-up to Devon

*Example document — this shows a scheduled email: `sent` stays null until it
goes out, when `stage` becomes `sent` and the schema requires the date. The
filename carries the planned send date. The setup skill deletes `*.example.md`
files after onboarding.*

To [Devon (Tools Weekly)](../people/devon-tools-weekly.example), replying to
[the pitch](2026-07-25-devon-pitch.example).

> Hi Devon, one nudge and then I'll leave you be. Since I wrote, a customer
> recommended PlainInvoice on HN unprompted, which is the kind of proof I'd want
> before featuring a tool. Happy to send anything that helps.

The only follow-up. If there's no answer by 2026-08-18, set Devon's stage to
`declined` with a note.
