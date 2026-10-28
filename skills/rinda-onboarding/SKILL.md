---
name: rinda-onboarding
description: Onboard a Rinda workspace so it can send — check what is missing, research and save the user's company profile with its sender address and products, upload the logo and brochure, create the sending mailbox (via rinda-mailbox), set up the email signature (via rinda-signature), and point to the web steps (plan). Use when the user first connects Rinda, says "start", "set up Rinda", "린다 시작하자", "우리 회사 정보 채워줘", or when any Rinda tool says the workspace is not ready.
---

# Rinda onboarding

Get the workspace ready to send: company profile, sender address (legally required) and products, logo and brochure, a sending mailbox, a signature, an active plan.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — it sets who you are, how you speak, and the rules for sending.

1. Call `get_setup_status`. `missing` lists what blocks sending; `recommended` lists what makes the emails better (products, signature, logo, brochure). Each item says whether you fill it with a tool or the user does it in the Rinda web app. Go through them in the order below, skipping what is already done.

2. **Company profile, sender address and products** (`company_profile` or `sender_address` missing, or `products` recommended) — one research, one confirmation, one save:
   - Call `get_workspace_overview` to see what is already saved, including products.
   - If you don't know the company's name or website, ask the user. Don't guess.
   - Research the company on the web: the official site, what it makes or sells, who it sells to, where it is based, roughly how big it is, its main phone number, its registered postal address — and its main products (name, one-line description, category, who buys it).
   - Show the user, together: the company values, the phone number, the **sender address** field by field (legal company name, address line 1, line 2, city, state/province, postal code, country), and the product list. Let them correct, add or drop anything.
   - About the sender address, say plainly that it is **required by law**: anti-spam rules such as the US CAN-SPAM Act require every sales email to show the sender's real postal address, so campaigns can't start without it. If you couldn't find it, ask the user for it.
   - Then call `set_company_profile` once, with `company_phone`, `sender_address` (in English/Latin script, country as a 2-letter code) and `products`. Pick `industry` from the allowed values only. Put a website in `official_website` only if you actually found it. If the tool says the website did not respond, tell the user it was not saved. If it says the product limit was reached, say how many were saved.

3. **Logo and company brochure** (`logo` / `brochure` recommended): ask once, "회사 로고와 회사 소개서(PDF 등)가 있으면 올려 둘게요. 파일 위치를 알려 주세요." For each file the user gives:
   - Call `create_upload_link` with `kind` (`logo` or `brochure`).
   - If you can run shell commands (Claude Code), upload it yourself: `curl -fsS -F "file=@<path>" "<upload_url>"`, and tell the user it's done. If the logo is only on their website, you may download it and upload that file.
   - If you can't run commands, give the user `web_fallback` and say where to upload it.
   - Skip if the user has none — it doesn't block sending.

4. **Mailbox** (`item: "mailbox"`): use the `rinda-mailbox` skill, then come back here.

5. **Signature** (`item: "signature"`): right after the mailbox, use the `rinda-signature` skill, then come back here.

6. **Web-only items** (`fix: "web"`, e.g. the monthly email quota or the plan):
   - Give the user the `url` from the status and say what to do there in one sentence.
   - When they say it's done, call `get_setup_status` again.

7. When `ready_to_send` is true and nothing is left in `recommended`, say so in one line, then use the `rinda-guide` skill to show what to do next.
