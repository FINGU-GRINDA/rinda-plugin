---
name: rinda-signature
description: Make or change the email signature that ends the user's Rinda emails — a version in the user's language and one in English, each shown rendered at the end of a sample email via a link, revised in conversation, then saved as the default. Use when get_setup_status recommends "signature", when the user asks for a signature ("서명 만들어줘", "서명 바꿔줘", "원래 쓰던 서명 넣어줘"), or when onboarding hands this step over.
---

# Rinda signature

Campaign emails end with the user's default signature. One signature holds a version per language, and buyers get the version in their language. Make two from the start — the user's language and English — and let the user see both exactly as buyers will before saving.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — it sets who you are, how you speak, and the rules for sending.

1. Call `list_signatures`.
   - A default exists and the user didn't ask to change it: show its text and languages, say campaign emails end with it, and stop.
   - The user wants to change one: show the current text, ask what to change, and continue from step 4.

2. Propose both versions in one message: "<user's language> 서명과 영문 서명을 같이 만들어 둘게요. 해외 바이어에게는 영문이 붙어요." (If the user writes in English, make only English unless they ask for another language.) Ask only for what Rinda doesn't know: the name to show in each language (e.g. 이예인 / Yein Lee), job title in each language, phone number (optional). Company name, website and address come from the saved company profile. If the user already has a signature they like, ask them to paste it.

3. Make the two drafts with `preview_signature`, one call per language (`language` = the user's language, then `en`):
   - **Default** — Rinda's generator: pass `sender_name` and the facts in `details` (e.g. "Title: Head of Sales, Phone: +82 10-1234-5678").
   - **Write it yourself** when the user pasted their existing signature or described an exact layout: pass your own `html` (inline styles and simple tables only; no scripts, forms or external CSS). Keep their wording as is; only translate for the other language.
   Send the user both `preview_url`s — each opens the signature at the end of a sample email in that language, as the buyer will see it — with the `signature_text` of each, and ask if anything should change.

4. For each change, call `preview_signature` again for the affected language(s) — same inputs plus the change (`instruction` for the generator, or your edited `html`) — and send the new links. Repeat until the user is happy with both.

5. When the user approves, save: first `save_signature` with the English `preview_id` (it becomes the default and is used when a buyer's language is unknown), then `save_signature` with the other `preview_id` and the returned `signature_id` to add that language. Tell them in one line that campaign emails will end with it, in the buyer's language.

6. If another skill sent you here (e.g. onboarding), go back to it; otherwise offer the next step from the `rinda` base skill's table.

Preview links expire after 30 minutes; if the user comes back later, make new previews. To see or manage all signatures later, give them `manage_url` from `list_signatures`.
