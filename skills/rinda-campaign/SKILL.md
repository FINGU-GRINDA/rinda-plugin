---
name: rinda-campaign
description: Create, review, test, start and change email campaigns (캠페인) in Rinda — from "누구에게 무엇을 보낼지" to sending, with every option the web campaign builder has (recipients, send times, mailboxes, signature, products, company document, attachments, ad label). Use when the user wants to send emails to buyers ("이 그룹에 메일 보내줘", "캠페인 만들어줘", "후속 메일 바꿔줘", "테스트로 나한테 보내봐", "캠페인 복사해서 다른 그룹에도"), or to check, pause, resume, postpone, copy or delete a campaign.
---

# Rinda campaigns

A campaign is 2–4 emails (steps) sent to buyer lists over days. Creating it sends nothing; starting it does. The web builder has many options — **don't walk the user through them.** Ask only what you can't decide, fill the rest with the web's defaults, and show what you used so the user can change anything in one sentence.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — especially the two-call rule for anything that sends, and showing the preview link.

1. **Who** — ask, unless already clear. `list_buyer_lists` and let the user pick one or more lists. If they want only some buyers ("독일만", "답장 없던 곳만"), find them with `search_buyers` and use `buyer_ids`. If no list fits, hand over to `rinda-buyers` first.

2. **What** — ask in one question: the goal of the emails and what to offer (product, sample, meeting…). Read `get_workspace_overview` for the company and products instead of asking what Rinda already knows. Pick the products this campaign sells (`product_ids`).

3. **Write** — draft 3 steps yourself in the buyers' language (from their country; ask only if the list mixes languages a lot):
   - step 1 on the start day, then 3 and 4 days later, at 10:00 buyer time — unless the user said otherwise
   - each step a different angle (introduce → value/proof → short last check-in), short paragraphs, one clear ask
   - `{{companyName}}`-style variables only where they help; don't invent variables
   - bodies are Markdown: **bold** and [links](https://…) sparingly; plain short paragraphs read best in cold email
   Show the steps compactly (subject + 2-line gist each) and adjust on feedback **before** saving.

4. **Save** — `draft_campaign`. Then show, in this order:
   - the **preview link** (opens without login, 30 minutes)
   - a **settings card** from the returned `settings`, 5 lines at most: recipients (lists, how many), send time (buyer local · times per step), from (mailbox), signature. End with one line: "바꾸고 싶은 게 있으면 말씀해 주세요."
   If the preview shows a buyer won't get an email (a variable that can't be filled), fix the draft first.

5. **Test** — offer once: "제 메일로 1번째 메일을 먼저 보내볼까요?" → `send_test_email` (goes to the user's own address).

6. **Start** — `start_campaign` (first call): show recipients — total, will send, excluded — and anything blocking. Ask start now or later. "Later" = set step 1's `delay_days`/`hour` with `update_campaign`, then start. Second call only after a clear yes.

## Options — offer only when the situation calls for it

| Situation | Offer | Tool field |
|---|---|---|
| Many buyers excluded because another campaign is emailing them | "그 바이어들에게도 보낼까요?" | `send_to_active_elsewhere` |
| Many excluded because they were emailed recently (30 days / 3 campaigns) | "최근에 받은 바이어에게도 보낼까요?" — mention it can feel like spam | `allow_recontact` |
| Buyers who already replied are in the chosen buyers | ask whether they should get it too | `include_replied_buyer_ids` |
| The user mentions a catalog, brochure or price list | the company document as a tracked link in email 1 (`list_brochures`), or a file attachment (`create_upload_link` kind `attachment` → `attachment_ids`) | `brochure_id`, `steps[].attachment_ids` |
| The user wants a product photo or picture in the email | upload it (`create_upload_link` kind `image`) and put `![description](image_url)` in the body | `steps[].body` |
| Several mailboxes in `list_mailboxes`, or a large list | send from several mailboxes in turn | `mailbox_ids` |
| Several signatures or languages in `list_signatures` | which signature / language version | `signature_id`, `signature_language` |
| Buyers in Korea and the email advertises | the legal ad label | `steps[].is_advertisement` |
| The user wants emails to go out at their own local time | change it | `send_time_basis` |
| The user wants to keep editing in the web app | save without making it startable | `save_as: "draft"` |

Never list these up front. One relevant question at a time, in plain words — not field names.

## Changing, checking, stopping

- **Change** anything with `update_campaign` (settings, step edits by number, add or remove steps) and show the new preview link. Steps can't be added or removed while sending — pause first. Buyer lists can't change after it has sent.
- **A step about to go out needs a fix** → `postpone_campaign_step` (e.g. 60 minutes), fix with `update_campaign`, done — the new text goes out.
- **Check** with `get_campaign` (settings and steps) and `get_campaign_stats` (sent, opens, replies, health score). Replies are handled with the inbox tools. "캠페인들 어때?" → one `list_campaigns` call already has each campaign's buyers, sent and replies.
- **Right after starting**, `get_campaign_stats` → `activation` tells whether buyers are still being added, it finished (`first_send_at`), or it `failed` (say the `error` plainly). Don't say emails are going out until it is `completed`.
- **Who got what** ("누가 몇 번째까지 받았어?", "A사는 어떻게 됐어?") → `list_campaign_recipients` (narrow with `buyer_name`, `replied`, `opened`; `excluded: true` for buyers left out at start and why). Answer with what was asked, not the whole table.
- **Stop emailing some buyers** ("이 회사는 빼줘") → `stop_campaign_recipients` with their buyer ids; the rest keep going. Say it can't be undone for this campaign. To stop everyone, pause instead.
- **Pause / resume** with `set_campaign_state` (resume sends again — two calls).
- **Same campaign for another list** → `copy_campaign` with that list, show its preview, then start it like a new one.
- **Delete** with `delete_campaign` (two calls) only when the user clearly wants it gone; otherwise pause.
