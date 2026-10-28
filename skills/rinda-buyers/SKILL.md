---
name: rinda-buyers
description: Manage the user's buyers and buyer lists (바이어 그룹) in Rinda — find buyers among all saved buyers, make a list, add or remove buyers, move them between lists, rename, merge or delete lists, and delete buyers. Use when the user talks about buyers, buyer lists or groups ("독일 바이어만 모아 그룹 만들어줘", "이 그룹 이름 바꿔줘", "두 그룹 합쳐줘", "그룹 없는 바이어", "바이어 삭제"), or before drafting a campaign when the list isn't ready.
---

# Rinda buyers and buyer lists

All saved buyers live in the workspace (전체 바이어). A buyer list (바이어 그룹) is a folder of some of them — a buyer can be in several lists, and campaigns send to a list. Deleting a list never deletes its buyers.

## Steps

0. **If the `rinda` base skill hasn't been read in this conversation, read it first** and follow it — it sets who you are, how you speak, and the rules for sending.

1. **See what's there** — `list_buyer_lists` for the lists and their sizes; `search_buyers` to find buyers among all saved buyers (keyword, country, status, one list, or `not_in_any_list` for buyers in no list yet); `get_buyer` for one buyer, including which lists it's in. Show results as a short table (company, country, status), not raw data.

2. **Do what the user asked**, using buyer ids from step 1 — never invent ids:
   - Make a list: `create_buyer_list` with a clear name and the buyers. If the user describes the buyers ("독일 유통사"), search first and show who will go in before creating.
   - Add or remove buyers: `update_buyer_list`.
   - Move buyers to another list: `move_buyers`.
   - Rename or describe a list: `rename_buyer_list`.
   - Merge lists: `merge_buyer_lists` — the merged-in lists are deleted; it's refused while a running or paused campaign uses any of them (say which, and that the campaign must finish or be archived first).
   - Delete a list: `delete_buyer_list` — buyers stay in all buyers.
   - Delete buyers for good: `delete_buyers` — only when the user clearly means removing them from Rinda, not just from a list. By default buyers in a running campaign are kept.

3. **Two-step actions** — deleting, merging, and any change that stops or starts emails (removing or moving buyers out of a list a campaign sends to, adding buyers to a list a running campaign sends to) return a confirm_token first instead of acting. Show the user what will happen in plain words — which list, how many buyers, which campaigns start or stop emailing them — and call again with the token only after a clear yes.

4. Confirm the result in one line (e.g. "독일 유통사 그룹에 23곳을 넣었어요"). If the user is building a list for a campaign, offer to draft the campaign next; otherwise offer the next step from the `rinda` base skill's table.

Finding new buyers that aren't saved yet happens in the Rinda web app's buyer search; here you work with buyers already saved.
