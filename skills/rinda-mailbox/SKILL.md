---
name: rinda-mailbox
description: Create the user's Rinda sending mailbox — an address on a Rinda mail domain (e.g. name@mail.rinda.ai) that sends their campaigns, with no sign-in or DNS work. Use when Rinda has no mailbox yet (get_setup_status lists "mailbox"), when the user asks for a sending address or "메일 주소 만들어줘", or when onboarding hands this step over.
---

# Rinda mailbox

Give the workspace an address to send from. Rinda mail works on every plan and is ready at once.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — it sets who you are, how you speak, and the rules for sending.

1. Call `get_setup_status`. If "mailbox" isn't missing, the workspace already has one — call `list_mailboxes`, tell the user which address sends (the default one), and stop unless they asked for another address. Otherwise take `suggested_prefix`, `default_domain` and `other_domains` from that item.

2. Propose one address to the user: `suggested_prefix@default_domain` (if there's no suggestion, offer something like their name or "sales"). Also ask for the sender name buyers will see — usually the person's name or the company name. Mention they can pick another prefix or one of `other_domains`.

3. When the user agrees (or gives their own prefix), call `create_rinda_mailbox` with `prefix`, `domain` if they chose a non-default one, and `display_name`.
   - Taken or not allowed (e.g. "admin", "support"): say so in one line and suggest a close alternative.
   - Blocked by plan or role: pass on what the tool says.

4. Confirm the new address in one line. If another skill sent you here (e.g. onboarding), go back to it; otherwise offer the next step from the `rinda` base skill's table.

If the user would rather send from their own company domain, that is set up in the Rinda web app — give them the `or_web` link from `get_setup_status`.
