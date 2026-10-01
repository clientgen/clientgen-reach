---
name: campaign-week
description: The advisor's marketing week at a glance, Phase 1 only. Triggers on "/campaign-week", "what marketing is scheduled", "what went out", "what is due this week", "marketing status", "show my campaigns". Pulls the calendar, recent orders, the compliance queue, and open drafts, and turns them into a short to-do list with the next action on each item.
---

# Campaign week

Read only, then propose. Nothing in this skill sends anything. It uses only the tools
Phase 1 ships: the calendar, orders, recipients, and the compliance review queue. Banner ads,
charitable donations and gift cards are not part of the ClientGen connector in Claude; if the
advisor asks, say they are handled in the ClientGen web desk (`/app`).

Run each tool and report what it returns; never predict what the calendar or the queue will
show before the call comes back.

## Steps

1. `get_campaign_calendar` for today through 14 days out (extend to 30 if the advisor asks
   for the month).
2. `list_orders` for `status: draft` (unfinished work), `status: pending_confirmation`
   (the advisor said yes and ClientGen emailed the one-click confirmation link, but it has not
   been clicked yet, so nothing is charged or mailed), `status: approved` and `queued` (in
   flight), and the last page of `status: sent` (what went out). `list_kits` with
   `status: pending_confirmation` finds kits waiting on their click the same way.
3. `list_reviews` with `status: open` for anything sitting in the firm's compliance queue,
   waiting on a reviewer before it can be approved.
4. `search_recipients` with `status: new` to count leads nobody has contacted yet.
5. Present one short list, most urgent first:

    This week
    - Wed 9/17  Seminar follow-up postcards (planned). 12 attendees, none mailed yet. Next: /postcard
    - Mon 9/22  October client touch (planned). Next: /handwritten-note
    In flight
    - Order 41  14 postcards, approved 9/11, queued
    Waiting on your reviewer
    - Ticket 6  postcard order 44, submitted 9/16, still open
    Waiting on your confirmation click
    - Order 45  22 thank-you postcards, $30.80. Click the link in your email; all pending items are at /app/approvals.
    Drafts waiting on you
    - Order 44  handwritten note to Susan Whitfield, $4.50. Send or cancel?
    Leads not yet contacted: 5 (3 from the 9/10 seminar)

6. Offer the next action on each line as a skill the advisor can invoke. Do not start any of
   them without being asked.
7. For a draft postcard with a page, `get_order` carries `action_check` when the card's words
   promise something its page does not do; list it on that draft's line with the fix it names.
   To restyle a draft card and its page together, `/postcard` and `/page-design` use
   `apply_shared_style`; every change returns fresh previews and keeps the earlier version.

## When the week is thin, propose from the season

If the next two weeks hold nothing, propose one or two touches that fit the date, with a reason
each, rather than just reporting an empty calendar:

| When | Touch |
| --- | --- |
| Mid to late January | Tax Season Reminder postcard to clients |
| March | Before You File postcard to clients |
| April to May | Thank You For Your Trust postcard to clients; seminar season invitations |
| June to August | Midyear check-in; plan fall seminars (invites go out three to four weeks ahead) |
| September to October | Fall seminar fill kit; income review campaign to prospects |
| First two weeks of November | Year-End Deadline Reminder to clients |
| November | With Appreciation notes to closest relationships |
| First two weeks of December | Warm Wishes For The Season postcard |
| Any market drawdown | A Word From Your Advisor to clients; Checking In notes to retirees drawing income |

Also scan recipient tags and notes for dates the advisor recorded (a tag like `birthday-oct`,
"client since 2019") and list the birthdays and anniversaries coming up in the next 30 days as
note suggestions. Never infer a date that is not recorded.

If the list itself is empty, offer to build it: the advisor pastes a sign-in sheet or shares a
CSV and you add it with `add_recipients`, first asking whether this batch is clients, prospects
or referral partners (a referral partner has no dedicated kind yet; store it as `lead` and say
so), passing `kind` explicitly, naming this specific list as the `source`, and asking the
advisor to confirm in their own words that they have permission to mail these people (the
`attestation`). Never a placeholder like "advisor list" or "unknown", and never a made-up
attestation.

## Never

- Never say a recipient's raw id to the advisor. Narrate people by name (and city when it
  helps tell two apart); ids are only for tool calls, never for a sentence a person reads.
- Ask at most two questions in one turn, and only the real unresolved business or
  compliance choices; group anything else for a later turn.
- Never end a turn with only a promise to send something later: when the advisor asks for a
  brief, a draft, or a list of questions, give it in full in this same reply.
