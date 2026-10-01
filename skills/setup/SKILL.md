---
name: setup
description: Set up and get started with ClientGen Reach, the advisor marketing connector for postcards, handwritten notes and hosted personal pages with a compliance record. Use whenever the advisor says "use ClientGen", "use Reach", "use ClientGen Reach", "set up ClientGen", "get me started", "what can ClientGen do", "connect ClientGen", "is ClientGen working", "first time", or asks what marketing they can send from here. Walks firm, brand, compliance contacts, connections and the skills map from live account state, offers a safe test draft, and marks setup finished. Also /setup.
---

# Setup

Reach requires a Reach account only. Never require an app.clientgen.ai subscription,
app login, or optional companion installation to finish this walkthrough.
`get_setup_status.products` reports current access. An unavailable optional app feature
is not a failed Reach setup step. The `/app` links returned here belong to the Reach
account desk, not app.clientgen.ai.

Use **"Set up ClientGen for me."** on any supported host. Slash commands such as
`/setup` are shortcuts only where the host supports them. If Reach tools are unavailable,
explain that the advisor must connect ClientGen Reach and sign in through its supported
authorization flow before setup can run. Do not ask for credentials in chat or invent
account state from uploaded skill files.

Run this once after connecting, and again whenever the advisor asks. Everything it reads is
live account state (`get_setup_status`, `get_brand_profile`, designs, recipients). The only
thing it writes is the "setup finished" mark. Firm data lives in ClientGen, not in this chat:
when something is saved, say "Saved in your ClientGen account." When something must be
changed on the web, give the page and say why the connector cannot write it.

## What the tool gives you, and what this skill adds

`get_setup_status` returns facts only. Its `walk` field carries `walk.mode` (`full` or
`already_done`), `walk.topics` (the topic titles, in order), `walk.summary` (one plain
summary per topic, built from live account state), `walk.skills_map` and
`walk.finish_message`. How to run the conversation lives here, in this skill:

- Call `get_setup_status` first, every time. Never walk from memory or an earlier chat.
- Take the topics in the order `walk.topics` gives, one or two per reply, and keep each turn
  short. Use `walk.summary` and the checklist for the facts and say them in plain words.
  Fill nothing in from memory. Do not collapse the walk into one status list.
- Never list raw tool names to the advisor anywhere in the walk. Use job language ("draft a
  postcard", "confirm it from the email").
- Never say the brand kit is approved unless `brand.status` says `approved`. Never write firm
  data from here; point to the web page named for it.
- If `brand.disclosure_missing` is true, say plainly "Add your firm disclosure before sending"
  and never call the brand or the brief complete. It does not block drafting.
- If `sender` says physical mail is not ready, say so plainly before offering any campaign. A
  finished walkthrough does not mean mailing is ready.
- Stop before any approval unless the advisor explicitly asks for one in their own words.
- Call `complete_setup` only after the skills map topic has been shown and the green, yellow,
  red summary is done. Then quote `walk.finish_message` exactly: "Setup is marked finished.
  Saved in your ClientGen account."
- When a summary names Claude, use the current host name for that label only. Treat slash
  skill names as workflow labels when slash commands are unavailable.

If `walk.mode` is `already_done` (topics: Already finished, Skills map, Next): say when setup
was finished (`setup_completed_at`, UTC) and that it is saved in their ClientGen account,
explain any sender or delivery blocker first, give the skills map, and suggest a first real
campaign (a seminar follow-up, or a postcard to a list they already have). Do not add a test
recipient again. Re-run a full topic only if the advisor asks. Otherwise walk the topics below.

## 1. Welcome

One paragraph: ClientGen Reach works without app.clientgen.ai and sends printed postcards,
handwritten notes, and hosted personal pages for booking, RSVP or replies. Every piece starts as a draft
with a real proof and an exact price. Nothing is charged, mailed or published from this chat:
once the advisor says yes, ClientGen emails them a one-click confirmation link, and only that
click (outside Claude) confirms the order. Every confirmed piece is written to a permanent
books-and-records archive the firm can hand to an examiner.

## 2. You and your firm

From `get_setup_status` (`advisor`, `firm`): confirm firm name, the advisor's name and title,
any DBA name, sign-in email, phone, website. Ask for what is missing. Phone, website and a
DBA are brand contact fields: they are saved at `/app/brand` (firm) or the advisor's own DBA
layer there, not from here. Read back what is on file and say where it lives.

## 3. Brand

From `get_setup_status`'s brand block: if `brief_status` is confirmed, say so in one line
(the version, and whether the kit it feeds is approved, waiting on a firm admin at
`/app/brand`, or rejected) and move on unless the advisor wants changes. Otherwise, even if
the visual brand kit is already approved, ask: "What is your firm's website? I will draft your
brand brief from it." Run `draft_brand_brief_from_website` with it. Share `review_url` for
the short brief and sample postcard, with `brief_url` available for the full research record.
Offer concrete desired-action examples (book a conversation, reply, RSVP or visit a page)
and the classic, modern and bold visual examples before asking which they prefer. Store
their choices in `response.default_action` and `look.visual_style`. Offer `color_palette_examples`
or custom colors at `review_url`, keeping official firm colors unless they choose a change.
Discuss colors in this conversation, using the read-only swatches at `review_url` as examples.
Accept exact hex codes or clarify the intended color, then save their chosen `look.primary_hex`
and `look.secondary_hex` with `update_brand_brief_draft`. Do not direct the advisor to a portal
design editor. The existing brand page handles logo uploads. Summarize the practice, audience,
main message, voice, desired action, style and colors briefly. Show the
sample before asking for confirmation, and name any remaining questions separately. Report `site_read` and `looks_like_own_firm` exactly as they
come back; never predict either before running the tool. Ask only the `open_questions` it
returns, at most two per turn, and apply each answer with `update_brand_brief_draft` as a patch
of only the field that changed. When the advisor says it looks right, call
`confirm_brand_brief`: if it returns `needs_firm_admin`, share `prefill_url` and say a firm
admin confirms it there; if it returns confirmed, say the brief is now the standing
instructions for how the firm sounds and what it says, while the print colors and logo still
wait on a firm admin pressing Approve brand kit at `/app/brand`. Close by saying what print
uses today (an approved kit only) and that pages can carry the brief's look right away.

If the firm has no website, say that is fine and follow `/brand-kit`'s "No website yet" path:
the firm admin types the name, colors, contact details and disclosure into /app/brand, and the
written brief waits until there is a site or the advisor dictates it. See `/brand-kit` for the
full flow (what a brand brief is, when `propose_brand_from_website` still applies) and its own
Never list; the same rules apply here: never dump JSON, never say a website read is a match
or an approval, never skip confirming, never invent a disclosure or a credential.

The brand brief's sound section holds the firm's standing voice. A page-specific tone
variation belongs in `/page-design`; it does not replace the standing voice. Ask only
about voice questions that remain open in the brief.

## 4. Compliance contacts

From `get_setup_status` (`firm`, checklist "Compliance contacts"): archive address (set and
verified, set but unverified, or not set), reviewer email, whether review is required before
every approval, and the gift cap. Then how approval works, in one line: every order and kit is
confirmed with a one-click link emailed to the advisor, outside Claude, and everything waiting
on that click is listed at `/app/approvals`. If the checklist says the email sender needs
setup, add that real confirmations cannot finish until it is set up. Changes to these settings
are made by a firm admin at `/app/settings`; the connector reads them, it does not write them.

## 5. Connections, honestly

From `get_setup_status` (`connections`): email sender, then each lane in job language: print
and mail, handwritten notes, hosted personal pages. Do not volunteer vendor names. If
`fulfillment_on` is false, say exactly this: pieces are confirmed and archived today, and
ClientGen releases physical mail and notes when it enables fulfillment; hosted pages are live
now. The web view is `/app/connections`.

Page address: read the checklist's "Page address" step and `page_address`. With an address, say
in one line that pages, QR codes and printed short links use it (for example
`lakeside.myfirmpage.com/<code>`). Without one, name `page_address.suggested_address` and say a
firm admin sets it on Firm settings (`/app/settings#page-address`); an advisor can ask for their
own at `/app/page-address`. Nothing in Claude sets or approves an address.

## 6. Skills map

Give `walk.skills_map`, one line each, in jobs language, never tool names:

- `/postcard`: pick a design and a list, see the real front and back, confirm by email.
- `/handwritten-note`: one-to-few pen-on-paper notes.
- `/page-design`: make a hosted page look like real marketing, change it on request.
- `/seminar-follow-up`: the week after an event, in one plan.
- `/brand-kit`: what the firm brand is and where it is set.
- `/admin-desk`: the web pages that prove a piece to a reviewer or examiner.
- `/setup`: this walkthrough, any time.

If the advisor asks about banner ads, charitable donations or gift cards: none of them is part
of the ClientGen connector in Claude. They are handled in the ClientGen web desk (`/app`). Say that in one line
and move on; never draft either here.

## 7. Optional test draft, only on a yes

Offer one safe test. If yes:

1. `search_recipients` for the advisor's own name or email first. Re-running setup must not
   add the same test person twice; reuse the row if it exists.
2. Otherwise `add_recipients` with the advisor's own home or office address, `kind: 'lead'`
   (a test row, not a real client or prospect), `source` "setup test, my own address", and
   their own attestation words.
3. Either `draft_page` (a book-a-call page, `design.use_default_look` true if there is no
   brand and they gave no colors) and give them the `preview_url`, or `draft_postcard` with
   an approved design and give them the proof link (`visual_preview.url`) with the price.
4. Stop there. Say what approval would do: it stages the draft and emails them a one-click
   confirmation link; nothing is charged or mailed until they click it, outside Claude, and it
   is not needed just to keep this draft on file. Do not call `approve_order` or
   `approve_kit` and do not coach the approval phrase unless they explicitly ask for a test
   approval, in their own words. If instead they say this was only to see it, or a demo,
   leave it right here as a draft.

## 8. Done

Summarize the checklist as green, yellow, red from `get_setup_status`, with the fix for each
yellow or red (the web page, or the person who owns it). If `counts.recipients` is one or two
(a lone test row, not a real list), lead with that before anything else: "let's get your real
client list in first, since a real campaign against one or two people is cosmetic, not a real
send". Offer `add_recipients` from a CSV, a pasted spreadsheet, or a CRM connector before
suggesting the first campaign. Otherwise propose the first real campaign from today's date, not a
generic "send a postcard": one sentence on the hook (for example "it's late September, so a fall
income review to your prospects, or year-end cards to clients in early November"), then a
three-touch sequence with dates in words, and ask which to start. Then call `complete_setup` and
quote `walk.finish_message` exactly: "Setup is marked finished. Saved in your ClientGen account."

## Design bar before any yes

A postcard is shown as its proof (a real card, front and back), a note as its message-side
proof, a page as its preview link. If a proof looks thin or generic, revise before asking
for approval: `/page-design` for pages, a new draft (with `replaces_order_id`) for mail.

## Never

- Never mark or describe a brand kit as approved from here.
- Do not volunteer vendor names. Say "ClientGen's print service" or "the note service" by
  default, and answer truthfully if the advisor asks who fulfills a piece.
- Never call `approve_order` or `approve_kit` for a test without the advisor's explicit words,
  and never describe a staged order as approved: only the emailed click confirms it.
- Never rely on chat memory for firm facts; read them again from the account.
- Never turn an advisor's own tenure, credentials or history into the firm's, or the firm's
  into one advisor's own.
- Ask at most two questions in one turn, and only the real unresolved business or
  compliance choices; group anything else for a later turn.
- Never end a turn with only a promise to send something later: when the advisor asks for a
  brief, a draft, or a list of questions, give it in full in this same reply.

## If ClientGen is not connected

Check before the first step. ClientGen Reach is connected only when its tools are available in
this conversation (for example `get_setup_status` and `get_brand_profile`) and a call to one of
them succeeds. Treat any of these as not connected: the tools are not listed; a call returns
"tool not found", an authorization or sign-in error, or a 401; or the host says the connector
needs to be connected.

When it is not connected, stop and say exactly this, in plain words:

"ClientGen Reach isn't connected in this conversation yet, so I can't see your account or
draft anything in it. To connect it: open your assistant's settings, go to Connectors, add a
custom connector with this address: https://mcp.clientgen.app/mcp, then choose Connect and sign
in with your work email. If your firm manages connectors for you, your administrator may need
to add it. Once it's connected, ask me again and I'll pick up from here."

Then add this skill's one line (below). Never invent account state: no brand status, recipient
counts, templates, prices, proof links, order numbers or approval status. Never draft, price or
"approve" anything as if connected, and never simulate a proof. You may help the advisor think
through the audience, the offer or the wording in plain text, labeled clearly as "not yet in
ClientGen", if they ask.

Once you're connected, I'll walk you through your firm, brand, compliance contacts and
connections in about ten minutes.
