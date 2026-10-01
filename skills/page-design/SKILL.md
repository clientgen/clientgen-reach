---
name: page-design
description: Build or restyle a hosted ClientGen Reach page (book a call, RSVP, reply) that the postcard's QR code or a shared link points to. Use for "make a booking page", "RSVP page for my event", "a page for this campaign", "welcome page for new clients", "partner page", "the page looks generic", "make it look better", "add my photo", "match our website", "change the colors on this page". Writes the page to match the piece that points to it, asks for a photo, never changes the firm brand by accident, and ends in a hosted preview, the advisor's own yes, and the emailed confirmation that publishes it. Also /page-design.
---

# Page design

The page is hosted by ClientGen at its own link. Never an artifact, never HTML pasted in the
chat, never a page drawn by hand. What the advisor sees in the conversation is a description and the
preview link; the truth is the hosted preview.

## Campaign review when available

If the result includes `campaign_workspace_url`, show it alongside the preview. The advisor
reviews and revises the design first, then prepares and downloads the exact PDF/ZIP for compliance
or explicitly emails a secure review link from that workspace. Follow `/marketing-compliance`'s
campaign-package workflow: independent approval of the current package comes before the owner's
separate final release. Material changes require a new package and decision. A private page can
be reviewed at its final address with a draft watermark and disabled forms; it activates only
after deliberate final confirmation. A requested date is planning information, not scheduling.
An expired release token can be renewed in the workspace without redrafting approved content.
This workflow takes precedence over the legacy approval steps below when the URL is returned.

## Think like a wealth marketer

You are the advisor's marketing lead, not a form filler. Before drafting anything, pin down four
things. Ask for whichever are missing, at most two questions per turn and the most important
first, never a questionnaire, and never ask for something the advisor already said.

1. **Audience.** Exactly who gets this and how they know the advisor: clients, seminar attendees,
   referral partners, or people who have never met the advisor. The same words cannot serve a
   client of ten years and a stranger from a purchased list.
2. **Offer or occasion.** The one reason this person hears from the advisor now: a thank-you, a
   deadline, an event, a complimentary review, a milestone. If there is no reason, there is no
   piece yet; say so and suggest one that fits the date.
3. **Timing.** When it has to land, in words ("the week of November 9"), counted back from the
   moment that matters (an event, a deadline, a birthday). Plan approval at least two weeks ahead
   of that moment unless the tools state a delivery time.
4. **One call to action.** The single thing the reader should do and where it happens: scan the
   code to book, RSVP or reply on the advisor's page. A life-event note usually has none, and
   that is correct.

Then say the plan back in one or two lines so the advisor can fix it in a word:
"Audience: the 38 people from the October 8 workshop. Offer: a complimentary income review.
Lands: the week of October 12. Ask: scan to pick a time."

## Match the piece that points here

Most pages are reached from a postcard's code or a link the advisor shares. The page finishes
what the piece started, so it must say the same thing:
- **Headline:** the offer in the piece's own words. A card that asks "Where will your paycheck
  come from?" leads to "Let's map out your retirement paycheck", not "Welcome to our firm".
- **Button:** the piece's CTA verb, and a button that really does it. Card says "pick a time";
  the page's button opens the calendar (`cta_action: book` with `scheduler_url`) and says "Pick a
  time". A label must describe what the button does: a reply form's button sends a note, so
  "Explore the demo" on it is refused with a truthful label.
- **Body:** delivers anything the piece promised. A welcome card that says the page lists "what
  to expect and who to ask" needs the real steps and the real names on the page, from the
  advisor. Never approve the piece before the page holds what it promises.
- **Audience:** a page for partners never reuses the client-facing main message; a page for a
  cold list never says "as our client".

## Write from the brand brief

Call `get_brand_profile` first. If `brand.brief.status` is `confirmed`, write the title and
body in its voice (`brand.brief.voice_summary`, `brand.brief.example_tone`), lead with
`brand.brief.main_message`, keep to `brand.brief.avoid`, and default the call-to-action label
to `brand.brief.default_action` unless the advisor asks for something else. If it is only a
`draft` or `none`, say so in one line and offer to finish the Day-1 brief (setup or
`/brand-kit`) before polishing the page, then write from whatever the advisor gives you. Never
copy the firm's website lines word for word, and never write a disclosure from the website.
`get_page_design`'s `brand` block says which colors the firm's cards and pages use and where
they came from (`colors_source`: `approved_kit`, `confirmed_brief` or `neutral_default`); a
brand kit on file but not approved yet shows only as `kit_on_file` and is not in use. Cards and
pages share these colors; the logo waits for kit approval. `design.colors_source` says where
THIS page's colors came from (`brand_kit`, `brand_brief`, or `page` for page-only colors). Say
which one it is when you show the preview.

## Two briefs, one rule

1. **Brand brief** (the firm's brand kit): logo, primary and secondary colors, contact block,
   disclosures. Read it with `get_brand_profile` or `get_page_design` (its `brand` block).
   It is the source of identity. This skill never writes it.
2. **Page design brief**: how THIS page uses that brand. Layout variant, tone, type pairing,
   headline size, spacing, the sections and their order, columns, hero layout, phone layout,
   form fields, what the main button does and its label, an eyebrow line, page-only colors,
   and the advisor's ask in their words. Stored on the page and hashed into the piece, so a
   design change is a content change: new preview, approve again. Every earlier version is
   kept.

The rule: a page can look different from the brand kit only when the advisor says "only this
page". A page never becomes the firm's brand by accident.

## Page copy by job

- **book:** title names the conversation ("A second set of eyes on your plan"); body says what
  the meeting covers, how long it takes and whether it is by phone, video or in person (all from
  the advisor); button "Pick a time" or "Book a review" only when the page opens the advisor's
  scheduler (`scheduler_url`). With no scheduler link, the form collects preferred times and its
  button reads "Send my times"; never promise a calendar the page does not open. Include the
  complimentary-meeting line when the meeting is complimentary.
- **rsvp:** title is the event title; body is what the evening or morning is, who is speaking,
  whether a guest is welcome; button "Save my seat". State that an educational event is
  educational and not an offer of advisory services.
- **reply:** title is a question the reader can answer ("What's on your mind?", "Who prepares
  your taxes?"); body says what happens with their answer and when the advisor will respond;
  button "Send a note" or "Send it to us".
- Every page: one headline, one ask, no testimonials, no performance, no pressure timing. Frame
  planning themes as questions or invitations. Never promise an outcome of the meeting.

## The interview, when the ask is thin

Ask only what is missing, at most two questions per turn, in this order:

1. A photo: a full https link to a .jpg, .png or .webp (the advisor, the office, the venue).
   No photo is fine; hero-form works without one - and if the advisor has already uploaded a
   headshot at `/app/headshot`, it shows automatically as a quiet hero accent even with no
   `image_url` set (check `get_brand_profile`'s `advisor.headshot` field). If they have no
   photo link handy, or their own website's link keeps failing the same-site check (common for
   a Squarespace/Wix/HubSpot site, which usually serves images from that platform's own
   address), suggest uploading a headshot at `/app/headshot` instead of chasing a website link.
   Never invent a photo link, and never accept a photo file or image bytes in this chat.
2. The one thing the visitor should do (the CTA verb): book, reserve, reply, RSVP.
3. Who the page is for (existing clients, seminar attendees, referrals), which sets the tone.
4. For a book job: the advisor's scheduling link (a Calendly or Google appointment page,
   https). Say plainly that the assistant's calendar connection is separate from ClientGen; the
   page links out to the advisor's scheduler, it does not read or write a calendar.

A `trust_line` ("Independent, fee-only, serving Jacksonville since 2009") is linted like
copy: facts only, no performance claims, no guarantees, no testimonials. Every fact in it comes
from the confirmed brand brief or the advisor's own words; never invent years, credentials or
"independent", "fee-only" or "fiduciary", and never restate one advisor's own tenure as the
firm's.

## Ask about the photo in plain words

A photo link alone is not enough; the page shows a photo only after the advisor has told you
it is theirs to use. Ask in plain language, never in field names, and pass only what they say:

- For any photo (`design.photo_is_own`): "Is this your own photo, or your firm's approved
  photo? And does it show any other company's name, logo or sign anywhere in the picture?"
  Set it true only when they say it is theirs and shows no other company's mark.
- On an RSVP page (`design.photo_is_venue`): "Is this the actual place where the event will
  be held?" Without a yes, the event page keeps its text-only hero.
- When `design_grade` comes back `review` because the link reads like a group or team picture
  (`design.photo_shows_advisor`): "Does this photo show you, or your own office, rather than
  a group or team picture?" Until they say yes, the page is not launch grade.

Never answer any of these for the advisor, and never infer them from the link or the file
name. A photo from a stock or public photo library cannot be used as their own; say so plainly
and point them to `/app/headshot` for their own photo. Photos are uploaded there, on the web;
never accept an image file or image bytes in this chat.

## Event dates and times

Write `event_time` out in words before calling `draft_page` or `revise_page` - "6:30 PM," never
a bare 24-hour clock or other shorthand - it is plain display text with no format check beyond
an 80-character cap, and it prints on the page exactly as given. `event_date` is different: it
must stay `YYYY-MM-DD` (only that shape drives the page's own auto-close at the event date,
below its 60-day default) - never rewrite it into words, or it is silently dropped from the
page entirely, along with that auto-close behavior. That said, the event block prints
`event_date` to visitors as the raw digits, which reads unfinished the same way an unformatted
postcard date does; there is no code path here to reformat it for display only. Say so plainly
if it looks off, and work around it in copy - spell the date out in the page's own title or
body ("Join us October 15, 2026") - rather than passing anything but `YYYY-MM-DD` to the tool.

## The photo rule (the difference between template grade and launch grade)

Every page result carries `page.design_grade`. `template` means no photo: the layout can be
warmer or form-first, but the page will still read as a form stub. When the advisor asks to
make it look better, redesign it, or make it launch-ready, ask for an https photo link before
you do anything else, and prefer `split-hero` (photo beside the headline) or `photo-top` once
you have it. If they skip, say plainly that the page stays template grade without a photo.
Never call a page done, finished, or launch-ready while `design_grade.grade` is `template`. For
a new-client welcome page or a meet-the-advisor page for a cold list, push once, in one
sentence, with the reason: "a photo of you or the office is what makes this read like your own
firm instead of a form, and for people who have never met you it is the whole first impression.
Want to add one?"; do not let it pass as merely mentioned once and dropped.

**Event pages, switching to a photo variant**: `split-hero` and `photo-top` are not guaranteed
to carry the event date, time, and seat count the way `event-rsvp`'s own layout does. After
`revise_page` moves an `event-rsvp` job to `split-hero` or `photo-top` for a photo, look at the
`preview_url` if you can open links (otherwise re-read the returned preview text) and confirm the event's date,
time, and capacity are actually visible before calling it done. If they are not, say so plainly
and offer to keep `event-rsvp`'s own layout instead of only adding the photo - never call a
page launch-ready on the strength of a photo alone.

A photo link being on file is not the same as the photo working. `design_grade.checks` only
confirms `image_url` was set, not that the link actually loads a real image. If you can open
links, look at the photo on the `preview_url` before calling a page launch grade; if you
cannot, ask the advisor to check it. If it is broken, blank, or clearly not what the advisor meant, say so
plainly, even when every check in `design_grade.checks` reads `ok: true`. A visibly broken
hero image reads worse to a visitor than the plain template look it was meant to replace.

**The advisor's own headshot counts as the photo.** On a book or reply page, `split-hero` or
`photo-top` with no `image_url` uses the headshot the advisor uploaded at /app/headshot as the
hero, cropped to the face. If they have none on file, the tool says so; point them to
/app/headshot rather than asking for a web link.

**Look at both crops before calling a page ready.** When a photo shows, the result's
`page.hero_preview` carries `desktop_url` and `mobile_url`: the hero with the photo cropped
exactly as the page crops it at each width. If you can open links, look at both, every time the
photo or the design changes, before calling the page ready or launch grade; if you cannot, give
the advisor both links and ask them to check the face in each. If a face is cut at an edge in
either crop, set `design.hero_focus` toward the face (top, bottom, left, right, center, or
"x% y%") with `revise_page` and look at both again.

**The crop is aimed at the face, and a cut face is `fix`.** With no `hero_focus`, the page aims
its crop at the face the photo check found, at desktop and phone widths. A `design_grade` of
`fix` means the crop the page actually renders cuts off the eyes or the top of a person's head
(usually a `hero_focus` set by hand); `approve_kit` refuses the page until it changes. Say the
reason plainly, then clear `hero_focus` (an empty string) or pick another photo, and look at
`hero_preview` again. Never talk past it.

When a supplied photo link fails the grade's check, offer a concrete alternative in the same
turn, never only "give me a different link" or "ship without one": ask for a different public
https link to a `.jpg` or `.png` with no query string (a link that ends in a tracking or
resize parameter is a common way a real photo still fails to load), or offer to proceed at
template grade with no photo for now. Either is a fine, honest next step; leaving the advisor
with only a dead end is not. Never call the result launch grade until a photo actually loads.

## Match a website

When the advisor names a site to match ("make it look like our website", "match
lakesidefp.example"), call `propose_brand_from_website` first. Then `draft_page` or `revise_page`
with `design: page_design_hint` from that result - it already carries the page-only colors,
fonts, logo, and tone with `color_scope: page_only` set, so it never touches the firm's brand
kit. Write the page's headline and body in the site's own voice: use `voice.tagline` and
`voice.headlines` as the model for the eyebrow, the headline, and the body copy - model the
voice, never copy it word for word (that would be copying a competitor's or the firm's own
existing site's copy, not writing the advisor's page). Tell the advisor plainly that the page
matches the site's look page by page, and that the firm's brand kit for mail is a separate
step, confirmed by a firm admin at the Brand page.

## The loop

1. `get_page_design(kit_id)` (or `get_brand_profile` before a first draft). Read the brand
   state and the current brief. Tell the advisor in plain words what the page uses today.
2. Translate the ask into the brief (`controls` in `get_page_design` lists every choice).
   Vocabulary that maps directly:
   - "make the headline larger / smaller": `headline_size` (smaller, standard, larger, largest;
     a title too long for a step on a phone is refused with the size that fits)
   - "tighten the spacing", "more room": `spacing` (compact, standard, airy)
   - "a friendlier font", "match the postcard's type": `typeface` (classic, modern, warm; the
     same three pairings the postcards print in, never another font)
   - "move the photo", "photo on the left", "photo behind the headline", "no photo up top":
     `hero_layout` (split-left, split-right, band-top, full-bleed, text); "crop to my face":
     `hero_focus`
   - "add a bio", "add FAQs", "put the form first", "drop the steps": `sections` (an ordered
     list of note, form, event, steps, about, faq, plan, photo, trust) with `about_text` and
     `faq`. The note and the form cannot be removed; say why (the form is how the page does
     its job, the note carries the advisor's own words) and offer a reorder instead.
   - "one column", "side by side": `columns` (1 or 2)
   - "on phones put the form first", "shorter hero on phones", "fold the FAQ on phones":
     `mobile.order`, `mobile.hero_height`, `mobile.collapse`
   - "don't ask for a phone number", "no party size": `form_fields` (only optional fields; a
     field the job needs always shows)
   - "change the button to Book a call and have it open my calendar": `cta_action: book`,
     `cta_label: "Book a call"` and the advisor's `scheduler_url`; "a button to our brochure":
     `cta_action: link` with the https `cta_url` they give (read it back to them; sign-in,
     payment and shortened links are refused); "a button that jumps to the FAQ":
     `cta_action: explore`, `cta_target: faq`
   - "form first", "form above the fold": `variant: form-first`
   - "it is an event", "RSVP page": `variant: event-rsvp`
   - "add my photo", "less template-y": `image_url` plus `variant: split-hero` (photo beside
     the headline) or `photo-top` (wide photo band); both need `image_url`
   - "warmer", "friendlier": `tone: warm`; "bolder", "punchier": `tone: bold`;
     "more traditional", "quieter": `tone: conservative`
   - "stronger call to action": `cta_emphasis: strong`, and a `cta_label` in their words
   - "darker navy", "use our green": `colors.primary_hex` (ask for the hex or a site to match)
   - "less stock", "less generic": pick a tone and a variant, add an `eyebrow`, tighten the
     title and body copy; there are no stock photos to remove, the look comes from the brand.
3. `revise_page(kit_id, design, title?, body?)` for a draft page, or `draft_page(..., design)`
   for a new one. Leave out anything the advisor did not ask to change.
4. Give them the `preview_url` (and `desktop_url` and `phone_url` to see both sizes) and the
   one-line `what_changed`. Say the earlier version is kept (`version`); `restore_version`
   brings one back. When the result carries `capped`, repeat the reason plainly. Do not paste
   HTML.
5. Before asking for that yes, look at `preview_url` if you can open links (otherwise re-read
   the returned preview text) every time the design or copy changes; never ask on the strength
   of the brief you just sent, even when every earlier check passed. Then say in one line what
   the proof actually shows (the headline, the event or the offer, the brand name on the page,
   and the disclosure or STOP footer) so the advisor is confirming the real page, not just
   answering a question.
6. The yes must be the advisor's own words for this exact preview; a yes to an earlier preview
   never carries over after `revise_page`. Call `approve_kit` with the new `preview_token` and
   their words as `advisor_confirmation`, exactly as they said them, never tidied. If the words
   are refused, ask them to say it again in their own words.
7. Say what happened, from the result: the page is staged, not live. Nothing is published until
   the advisor clicks the one-click confirmation link emailed to them, outside Claude. Pending
   items are listed at `/app/approvals`; a demo account gets the link in the result as
   confirm_url instead, to open in a browser. Never say the page is live at this point.
8. When they say they clicked, check `get_kit` and report its status. Once it is approved, look
   at the live `page.url` too if you can open links (or ask the advisor to), not only the
   draft-time preview, since a bug that only shows up once a page is live (a logo, a broken
   image) is worth catching and saying plainly before the advisor finds it. Then say in one line
   what the firm's CCO can already see from chat today: `list_reviews`, `list_archive`,
   `get_archive_record`, `get_order`, and `get_review_status`, all firm-wide for a firm admin,
   and that deciding a still-open review ticket only happens at the `/review` web page, not
   from chat.

## When the brief is thin (no brand kit, no colors)

`draft_page` and `revise_page` refuse with the intake questions until you have one of: page
colors from the advisor, their okay to use the plain ClientGen look
(`design.use_default_look: true`), or a confirmed brand brief with a color on file - that last
one defaults the page's look automatically (`design.colors_source: brand_brief`), so no
questions come back. If the brief is only a draft, the intake still fires, with one added line
offering to finish the brief first. Ask only what is missing, at most two questions per turn,
in this order:

1. Primary color (a hex; if the advisor gives a website, use propose_brand_from_website for suggestions).
2. Secondary or accent color, if any.
3. Is a logo on file? If not, they upload it at `/app/brand`; the page shows it once approved.
4. Tone: conservative, warm, or bold.
5. Contact lines that must appear.

Build the brief from the answers and draft. Then offer, as a separate yes: "Want these colors
saved as the firm's brand kit? That is done at /app/brand and goes through the brand check."
Never say the brand kit is set because the page uses those colors.

## When the ask conflicts with the brand kit

If the firm has an approved brand kit and the advisor wants a different primary or secondary
color on the page, the tool refuses and returns the question to ask. Ask it plainly:

"That is different from your brand kit (today: primary #112233). Update the brand kit for the
whole firm, or only this page?"

- Firm-wide: point them to `/app/brand` (admin desk). The brand check runs there. Nothing in
  this connector changes the firm brand.
- Only this page: call again with `design.color_scope: "page_only"` and the colors.

Dropping the disclosures, the opt-out link, the form or the advisor's note is not a design
option. Say so and offer a layout change instead.

## Fixed lines follow the page's words

The page's own fixed lines (the hero's one-line standfirst, "How this works", the form's helper
text) follow the page's job and never contradict the advisor's body. When the body says nobody
will follow up (a sample or test page), those lines make no promise of a reply, a call or a
mailing, and `get_page_design` says so in `fixed_copy_note`. Read the preview for any line that
still contradicts the body and say so plainly.

## One style for the card and its page

For "apply this style to both pieces", "match the page to this postcard" or the reverse, use
`apply_shared_style` with the postcard (`order_id`, it must point at this page with
`page_kit_id`) and `direction` (both, card_to_page, page_to_card). It carries the type pairing,
headline size, spacing, colors and photo focus, anchored on the confirmed brand brief, and names
anything one piece cannot take in `capped`. Colors that differ from the firm colors need the
advisor's okay first (then pass confirm_off_brand as true). Show both previews and what changed on each; each
piece is approved on its own.

## Live pages

A live page is not revised in place (its link is already out). Say that, then `draft_page` a
new one with the revised brief; it gets a new link. The old page closes on its own date, or
`cancel_kit` closes a draft or a page still waiting on its confirmation click.

`get_kit` says when a page has closed (`page.closed_at`, after its own closing date or when a
firm admin closed it early): tell the advisor plainly that it is closed and since when. For
each recipient's own QR code, pass `page_qr_recipient_id` for one person at a time.

## Variants (all hosted, branded, mobile-solid)

- `hero-form`: color hero band with the headline and lead, the copy, then the forms. Default
  for book and reply pages.
- `form-first`: slim header, the form card right under the headline, the copy after. Short asks.
- `event-rsvp`: event block (date, seats) with the RSVP form up top, the copy below. Default
  when the page has an rsvp job. The RSVP form collects a name, yes/no, party size, and an
  optional email; a `book` job's form also takes an optional phone. Either is only ever read
  back by the advisor's own `list_page_responses` call - never echoed on the public page, never
  used to contact anyone automatically. Say this plainly: ClientGen does not email, text, or
  otherwise message a registrant itself beyond the one-line on-page thank-you; the advisor is
  the one who follows up, by reading responses with `list_page_responses` and reaching out
  themselves. For an audience that is not already all saved contacts (a public link, a cold
  list), also say: a respondent who is not already a saved recipient still shows up in
  `list_page_responses` by name (and email or phone, if they gave one), but has no mailing
  address on file, so a follow-up postcard or note is not possible without asking for one.
  Offer to add the invite list as recipients first (so responses attribute to a known person
  with contact details on file) rather than relying on the public form alone.

Tones: `conservative` (serif headline, quiet palette), `warm` (soft tint, rounded, friendly),
`bold` (heavy headline, uppercase eyebrow, full-width button).

## Never

- Never produce the page as an artifact or as HTML in the chat.
- Never change the firm's brand kit from here, and never say it changed.
- Never remove disclosures, the STOP footer, or the contact line.
- Never approve for the advisor. Their yes, in their words, every time the preview changes,
  and never call a page live until their emailed confirmation click has gone through.
- Never say a recipient's raw id to the advisor. Narrate people by name (and city when it
  helps tell two apart); ids are only for tool calls, never for a sentence a person reads.
- Ask at most two questions in one turn, and only the real unresolved business or
  compliance choices; group anything else for a later turn.
- Never end a turn with only a promise to send something later: when the advisor asks for a
  brief, a draft, or a list of questions, give it in full in this same reply.

## If ClientGen is not connected

If ClientGen Reach tools are not available in this conversation, or a call returns "tool not
found", a sign-in error or a 401, follow `/setup`'s "If ClientGen is not connected" steps: say
plainly that it is not connected and how to connect it (a custom connector at
https://mcp.clientgen.app/mcp, then sign in), and never invent account state, prices,
proof links, order numbers or statuses, or draft anything as if connected. Then add:

Once you're connected, I'll build the page and give you a preview link before anything goes
live.
