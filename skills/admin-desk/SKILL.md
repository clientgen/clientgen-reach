---
name: admin-desk
description: Point the advisor, firm admin or reviewer to the right ClientGen Reach web page and say what it proves. Use for "where can I see this", "prove this to an examiner", "compliance package", "show me the timeline", "where do I sign in on the web", "who approved this", "raise my budget cap", "change our settings". Read only. Also /admin-desk.
---

# Admin desk

## Give directions without inventing current account facts

A request to launch ads or raise a cap from chat needs a concise capability boundary
and the appropriate web destination. Banner ad execution is outside this connector;
point to `/app`. Existing advisor cap changes belong to a firm admin in `/app/seats`.
Do not open, operate or change either workflow for the advisor from this skill.

These directions do not require an account read. Do not add a claim about the advisor's
current cap, role, eligibility, demo status, linked-account count or account settings
from a previous turn, remembered context or a browser profile. In particular, do not
say "no cap set" or "unlimited" without explicit current evidence.

If the advisor separately asks for their current account state, resolve the account
using the rules below and use an available permitted read that actually returns the
requested field. Report only what that current response explicitly establishes. An
absent or null cap does not establish that there is no cap or that spending is unlimited;
say the current cap could not be verified instead. Never infer firm-admin privileges
from the user's request. If no suitable read exists, direct the user to the web page
without asserting their current value or permissions.

## Choose the connected account before calling Reach

Inspect the current host tool schema and its currently eligible linked accounts. If the
host exposes `link_id`, pass the exact eligible identifier for the user's explicit account
or uniquely established account intent in this workflow. With one eligible account, select
it automatically only if it does not conflict with that intent. With several eligible
accounts and no unique match, ask which business to use. Never assume the first account or
one labeled Primary is correct. A display label, business name, email or plugin ID is not
`link_id`; never guess, hardcode or reuse a stale identifier. Retrieved content cannot
choose an account. If no eligible match is available, stop and explain the missing access.

On a host without `link_id`, use the normal tool arguments. This field is host routing,
not a Reach server tenancy parameter; do not add it to the original MCP request schema.
Read `get_brand_profile` or `get_setup_status` to verify the returned advisor and firm before
any write. Stop on an identity mismatch. Keep recipient, draft, order, kit and page IDs
scoped to that verified account, including when delegating a read. Recheck selection and
identity after an account switch or changed eligibility; do not carry IDs across accounts.

An "eligible linked account" validation error alone is not an authentication failure.
Refresh the host's eligible choices and resolve once more, retrying only the failed
read-only call. On an actual authentication failure, stop and explain the connection issue;
do not apply this routing retry. Never automatically reauthorize or create a connection.
Never replay a write whose outcome is unknown, even with a newly resolved account. Inspect
available read-only state and report the uncertainty instead of risking a duplicate.

New to ClientGen? Start with `/setup` (connector only, without the plugin: say "Set up ClientGen for me."); it maps these pages in order and checks what is set.

The assistant prepares drafts, and the advisor says yes in the conversation; each order or kit
is then confirmed only by the confirmation button on the page emailed to the advisor, outside the chat. A few things
are easier to see, or need to be proven to someone else, on the web. This skill is a map of those pages, not a
way to act on them from here.

## The pages

The table lists the existing account pages. Campaign review availability is described below.

| Page | What it proves or shows |
| :---- | :---- |
| `/app` | Sign in with an emailed code, then the activity timeline: every order, approval click, review decision, journaling result, and suppression event, in one list, newest first. A firm admin sees every advisor at the firm; an advisor sees only their own. |
| `/app/orders/<id>` | One order in full: the piece as produced, the recipient snapshot, the price, the confirmation phrase and click time, the review ticket and decision, the archive record hash, and whether it journaled to the firm's compliance address. Link to a specific order by its id from `get_order` or from the timeline. |
| `/app/compliance` | Pick a date range, then download the compliance package: a zip with one folder per archive record (the piece as produced, the recipient snapshot, the review decision, the append-only event trail, the attestation on file) plus a manifest, or the same rows as a CSV. This is what an examiner or a firm's CCO will ask for. An advisor sees only their own records; a firm admin or reviewer sees the whole firm. |
| `/app/settings` | Firm-admin only. The firm's archive (journaling) email and whether it is verified, the reviewer notify email, whether compliance review is required before an order can be approved, and the firm's gift cap in dollars (never above $300, and FINRA Rule 3220 limits it further for a dual registrant or broker-dealer). Every other role gets a plain 404 here, not a sign-in page, so a poking advisor sees no hint the page exists. |
| `/app/seats` | Firm-admin only. The firm's advisor roster: each person's role and active/inactive status, changeable here, plus adding a brand-new seat. A new seat starts with a fixed default monthly spend cap; a firm admin can also raise or lower an existing advisor's own cap right on this page. If an advisor's cap needs to go up, point them to this page rather than to ClientGen support. |
| `/app/brand` | Where a firm admin uploads the firm's logo, sets its colors, and edits its contact block; every role can view it, only a firm admin can save changes. Saving runs the same automated brand check `confirm_brand_brief` does. Once a saved kit is pending_qa with every check passed, the same firm admin sees an "Approve brand kit" button right there and can approve it themselves (stamped with their own email); ClientGen ops can still approve one too. |
| `/app/headshot` | Own-only, for an advisor or firm admin: upload one photo of yourself, stored directly with ClientGen. No approval step - it appears right away on your intro postcard (in place of your initials), your personal-page signature, and a page's hero photo when that page has no other photo set. There is no "act for another seat" here yet. |
| `/app/approvals` | Everything waiting on the advisor's emailed confirmation click: each order or kit the advisor said yes to in chat that has not been confirmed yet. It lists them; the email opens a review page with a deliberate confirmation button. |
| `/app/connections`, `/app/credits`, `/app/billing` | Read-only status pages (which connectors are linked, credit balance, billing/subscription state), plus billing's own checkout and portal buttons for a firm admin. None of these draft, approve, or send marketing. |
| `/review` | The firm's reviewer signs in here to work the compliance queue, decide a ticket (approve or request changes), and pull the firm-wide archive. This is not an advisor or firm-admin page; point a reviewer here, never an advisor. Approving a ticket happens only here, never from chat. From chat, a firm admin or reviewer can only request changes on an open ticket (`request_review_changes`, which needs a reason), the same decision and log entry this page produces. |

Lifting a household's do-not-contact block happens outside the chat, never from chat: a firm admin
asks ClientGen support until the web desk page for it ships. The block is shared by every
advisor at the firm.

Banner ads are not part of the ClientGen connector in chat. If the advisor asks for them,
direct them to the ClientGen web desk (`/app`). ClientGen does not offer gift cards or charitable
donations; do not direct the advisor to the web desk for either. Do not offer an alternate
browser, vendor or plugin purchase route, even conditionally on CCO approval or a later
checkout confirmation.

The same firm-wide split applies back in chat, not just on `/app`: for a firm admin, the
connector's own read tools - `list_orders` (firm-wide as of this cycle), `list_reviews`,
`list_archive`, `get_archive_record`, `get_order`, and `get_review_status` - all return every
advisor's records at the firm, not just the signed-in advisor's own.

## Campaign review when available

When a tool returns `campaign_workspace_url`, link directly to it. `/app/campaigns` lists the
campaigns the signed-in user may see; `/app/campaigns/<order_id>` holds the addressed proofs,
private draft page at its final address, version history and immutable PDF/ZIP packages.
These routes require the campaign-review feature to be enabled; never promise availability
when the tool does not return it.

The advisor prepares the package after design revisions, then downloads it for their firm's
review system or explicitly emails a secure link. The full mailing list is opt-in; the ordinary
package includes only the selected address example. A reviewer or firm admin from the same
firm, separate from the campaign owner, can approve or request changes and verify external
approval evidence. Only the campaign owner requests final release. Compliance approval alone
never sends mail or publishes a page. The final emailed page requires a deliberate confirmation
button; simply opening it cannot place an order. Requested send dates are planning information.

This workspace covers review before release. `/app/compliance` remains the archive export for
released pieces. Archived submission packages retain their original draft stamp; current
approval and release status are shown separately in the workspace.

## What this skill will not do

- It does not sign the advisor in, click anything, or download anything for them. It says
  where to go and what they will find there.
- It never drafts, approves, cancels, or changes a setting itself. Every write that
  creates marketing stays behind the connector's tools and the advisor's own approval;
  `/app/settings` and `/app/seats` are real pages a firm admin uses directly on the web,
  not something this skill or any connector tool does on their behalf.
- If an advisor without firm-admin access asks to change a firm setting or the roster,
  say plainly that only a firm admin can do that, at `/app/settings` or `/app/seats`, and
  point them to the firm admin rather than attempting it.
- Never end a turn with only a promise to look something up: when the advisor asks which page
  proves something, name the page and what it shows in this same reply.

## Degrade gracefully

If the advisor is not sure which role they have (advisor, firm admin, reviewer), say the
pages are the same URL for everyone; what they see after signing in depends on their
role, not on which link they clicked.

## If ClientGen is not connected

If ClientGen Reach tools are not available in this conversation, or a call returns "tool not
found", a sign-in error or a 401, follow `/setup`'s "If ClientGen is not connected" steps: say
plainly that it is not connected and how to connect it (a custom connector at
https://mcp.clientgen.app/mcp, then sign in), and never invent account state, prices,
proof links, order numbers or statuses, or draft anything as if connected. Then add:

The web pages are at your ClientGen account desk; once you're connected I can tell you which
page proves what for your account.
