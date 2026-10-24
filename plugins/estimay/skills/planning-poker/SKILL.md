---
name: planning-poker
description: Use when the user wants to plan or estimate a sprint, set up a planning poker (scrum poker) room, import backlog stories for estimation, check who has voted, or record agreed estimates in their tracker, with Estimay.
---

# Planning poker with Estimay

Estimay runs planning poker rooms. Its MCP server (`estimay`) acts as the person who connected it, as the guest of their browser, so rooms you create open in that browser with them as host. If its tools ask for sign-in, tell the user to run `/mcp`, pick **estimay** and allow it in the browser.

## The order of play

1. **Before the meeting: prepare.** Gather the stories (from the user, or from their tracker's own MCP tools), keep each title to one line and keep the tracker key as the ticket. Show the list and ask what to add, drop or reorder. Then call `create_room` once with all the stories, and give the user the room link to share. People join in their browser with no sign-up.
2. **During the meeting: stay out of the vote.** The team votes in the room, in secret, and the cards turn over together. Never suggest an estimate before the reveal, and call `vote` only when the user tells you which card to play for them. You can still help run it: `start_voting` (the next story, or one by id), `reveal`, `revote`, `start_timer`, `add_stories` for a story someone just remembered.
3. **After the reveal: record it.** `set_estimate` saves the agreed estimate when the team settles on another card than the result. `get_room` shows the results, including how far apart the votes were; raise wide spreads as questions for the team, not as judgements.
4. **After the meeting: write it down.** `export_results` gives a Markdown table with the total (or CSV). If the stories were imported from Jira, Linear, Trello or Asana, `write_estimates` sends every estimate back through the room's own connection; otherwise update the issues with the tracker's own tools.

## Tips

- Every tool takes a room's 5-character code or its link.
- Stories are named by id in `get_room`; `edit_story`, `delete_story`, `start_voting` and `set_estimate` take that id.
- Adding stories, voting rounds, settings and estimates need the host or a facilitator; `set_facilitator` (host only) takes the person's name as `get_room` lists it.
- A room someone else created needs `join_room` first (with its password, if it has one).
- `end_room` deletes the room for everyone. Export the results first, and only end it when the user asks.
- Errors come with what to do next ("Call join_room first."). Follow them rather than retrying blindly.
