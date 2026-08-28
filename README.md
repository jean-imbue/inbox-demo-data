# Inbox demo data

A fabricated dataset of **65 messages** (50 email, 15 Slack)
for developing and demoing a unified inbox.

Every message here is made up. There are no real people, addresses, companies,
or conversations in it, and it was never derived from anyone's mailbox. It
exists so an inbox UI can be built and shown without touching real mail.

## What it covers

The set is written to exercise the shapes an inbox actually has to handle, not
just the common ones:

- **Both sources.** Email with real HTML bodies, and Slack messages with
  channels, DMs, externally-shared channels, and reply counts.
- **Six triage buckets**, the ones a person sorts by rather than the ones a mail
  provider labels by:

| Bucket | Count | What it means |
|---|---|---|
| Need response | 10 | A person is waiting on you |
| From boss | 7 | Sent by your manager |
| External | 8 | A real person outside your company |
| Ack | 3 | Worth seeing, nothing owed |
| Alerts | 24 | A machine sent it |
| Unimportant | 13 | Recurring bulk mail from strangers |

- **Edge cases worth testing:** messages with attachments, multi-message
  threads, pending reminders (including one already overdue), package and
  flight status data, senders with and without a `List-Unsubscribe` header, and
  HTML bodies ranging from a plain paragraph to a full marketing template.

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
| `unread`, `message_count` | |
| `attachments` | `name`, `size`, `type` |
| `remind_at` | ISO timestamp, or `null` |
| `unsubscribe_url` | Present only where a bulk sender would set the header |
| `source_url` | Deep link back to Gmail or Slack |
| `channel`, `channel_kind` | Slack only: `channel`, `dm`, or `connect` |
| `assist` | Optional package/flight status |

`now` at the top of the file is the reference clock the data was written
against (2026-08-27T16:40:00-07:00). Relative labels like "Today" and "Reminder due" only read
correctly if you treat that as the current time rather than the real clock.

## Licence

Public domain. It is invented data; do what you like with it.
