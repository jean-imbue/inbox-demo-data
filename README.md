# Inbox demo data

A fabricated dataset of **117 messages** (50 email, 67 Slack)
for developing and demoing a unified inbox.

Every message here is made up. There are no real people, addresses, companies,
or conversations in it, and it was never derived from anyone's mailbox or
workspace. It exists so an inbox can be built and shown without touching real
mail.

## What it covers

The set is written to exercise the shapes an inbox actually has to handle, not
just the common ones.

**Six triage buckets** -- the ones a person sorts by, rather than the ones a
mail provider labels by:

| Bucket | Count | What it means |
|---|---|---|
| Need response | 17 | A person is waiting on you |
| From boss | 13 | Sent by your manager |
| External | 13 | A real person outside your company |
| Ack | 20 | Worth seeing, nothing owed |
| Alerts | 36 | A machine sent it |
| Unimportant | 18 | Recurring bulk mail from strangers |

The pair worth getting right is Ack and Unimportant. Ack is "I should see this";
Unimportant is "I should never have received this". A colleague joking in
`#random` is Ack. A vendor's third cold email this month is Unimportant. A
system that collapses those two either buries your team's chatter with spam or
refuses to let you unsubscribe from anything.

**Slack, in the three shapes it comes in** -- 44 channel
messages, 16 direct messages, and 7 in
channels shared with another company. Only 17 of the 67 name
you; the other 50 are ambient chatter, which is the ratio
that makes triage hard and the reason `mentions_you` exists as a field.

**Edge cases worth testing:** attachments, threads with reply counts, pending
reminders including one already overdue, package and flight status, senders with
and without a `List-Unsubscribe` header, an externally-shared channel where the
sender is still a colleague, and HTML bodies ranging from one paragraph to a
full marketing template.

## Shape

Each message is a flat object. The fields that carry meaning:

| Field | Notes |
|---|---|
| `id`, `thread_id` | Stable identifiers |
| `source` | `email` or `slack` |
| `bundle` | One of the six buckets above |
| `from_name`, `from_email` | Sender; Slack uses an `@handle` |
| `subject`, `snippet` | Slack has no subject, so it repeats the channel |
| `body_html` | Renderable HTML |
| `date` | ISO 8601 with offset |
| `unread`, `message_count` | `message_count` is thread replies |
| `attachments` | `name`, `size`, `type` |
| `remind_at` | ISO timestamp, or `null` |
| `unsubscribe_url` | Present only where a bulk sender would set the header |
| `source_url` | Deep link back to Gmail or Slack |
| `channel`, `channel_kind` | Slack only: `channel`, `dm`, or `connect` |
| `mentions_you` | Slack only: were you named |
| `assist` | Optional package/flight status |

`now` at the top of the file is the reference clock the data was written
against (2026-08-27T16:40:00-07:00). Relative labels like "Today" and "Reminder due" only read
correctly if you treat that as the current time rather than the real clock.

## Licence

Public domain. It is invented data; do what you like with it.
