---
name: rinda-guide
description: Show the user how to use Rinda from this chat — what Rinda does, what they can ask for here, and the one next step that fits their workspace right now, with a few example requests to try. Use when onboarding has just finished, or when the user asks what Rinda can do or how to use it ("린다로 뭐 할 수 있어?", "어떻게 써?", "사용법 알려줘", "what can you do").
---

# Rinda guide

Teach by the next step, not by a feature list. One short message.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — it sets who you are, how you speak, and the rules for sending.

1. Say what Rinda does in one line, in the user's language — e.g. "린다는 해외 바이어를 찾아 그룹으로 묶고, 바이어 언어로 메일 캠페인을 보내고, 답장까지 챙겨요."

2. Call `get_workspace_overview` and pick the **one** next step that fits where they are:

   | Where they are | Next step to offer |
   |---|---|
   | No saved buyers | Finding new buyers happens in the Rinda web app's buyer search — give the link (the app's address + `/lead-discovery`) and a sample search, e.g. "독일 화장품 유통사". Once found, they can work with them here. |
   | Buyers, but no buyer list | Gather buyers into a list — e.g. "독일 바이어만 모아서 그룹 만들어 줘" |
   | A list, but no campaign | Draft a campaign for it — e.g. "이 그룹에 보낼 캠페인 써 줘" |
   | Campaigns running or done | Check replies and results — e.g. "답장 온 거 있어?", "캠페인 성과 보여 줘" |

3. End with **three** example requests they can type next, chosen for their situation (one of them the step above). Short, in their language, in quotes. For example:
   - "그룹 없는 바이어 보여 줘"
   - "남은 사용량 알려 줘"
   - "답장에 초안 써 줘"

Don't list every tool or explain the setup again. If they ask "what else?", group the rest briefly by job — buyers and lists, campaigns, replies, settings (company info, mailbox, signature), plan and payment, contacting the Rinda team — one line each, then stop.
