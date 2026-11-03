---
name: planning-poker
description: Use when the user wants to plan or estimate a sprint, set up a planning poker (scrum poker) room, import backlog stories for estimation, check who has voted, or record agreed estimates in their tracker, with Estimay.
---

# Planning poker with Estimay

Estimay runs planning poker rooms. Its MCP server (`estimay`) acts as the person who connected it, as the guest of their browser, so rooms you create open in that browser with them as host. If its tools ask for sign-in, tell the user to run `/mcp`, pick **estimay** and allow it in the browser.

## The order of play

1. **Before the meeting: prepare.** Gather the stories (from the user, or from their tracker's own MCP tools), keep each title to one line, the tracker key as the ticket and each item's link. Show the list and ask what to add, drop or reorder. Then call `create_room` once with all the stories (with `copy_from` set to last sprint's room to keep its cards and settings), and give the user the room link to share. People join in their browser with no sign-up.
   If the stories are Jira, Linear, Trello, Asana or GitHub items and the user wants the estimates written back there, create the room without them, ask the user to connect the tracker in the room (Stories ⋯ → Import from a tracker), then call `import_stories` with the items' links: they come in linked, so `write_estimates` can write back later.
2. **During the meeting: stay out of the vote.** The team votes in the room, in secret, and the cards turn over together. Never suggest an estimate or an answer to an extra question before the reveal, and call `vote` only when the user tells you which card to play for them. You can still help run it: `start_voting` (the next story, or one by id), `reveal`, `revote`, `start_timer`, `add_stories` for a story someone just remembered.
3. **After the reveal: record it.** `set_estimate` saves the agreed estimate when the team settles on another card than the result. `get_room` shows the results, including how far apart the votes were; raise wide spreads as questions for the team, not as judgements.
4. **After the meeting: write it down.** `export_results` gives a Markdown table with the total (or CSV). If the stories were imported from Jira, Linear, Trello, Asana or GitHub, `write_estimates` sends every estimate back through the room's own connection; otherwise update the issues with the tracker's own tools.

## Sitting in as an AI participant

Only when the user asks you to sit in on the vote yourself: `add_ai_participant` (host or facilitator; pass your name, like `Claude`) gives you a seat of your own, marked AI, and `ai_vote` plays your own card, a short reason and, in `answers`, your answers to the room's extra questions on each story. This is the one exception to staying out of the vote.

- Make it your own judgement, from the story: read an imported story through its `link` in `get_room` with the tracker's tools if you have them, otherwise go by its title and discipline. A split story has an id per discipline; vote on each for that discipline's work. `?` is an honest card when a title says too little.
- Vote on every story not revealed yet, in one call: your card is down when each reaches the table, so nobody has to come back to you mid-meeting. A story skipped or voted again loses your card; vote on it again when asked.
- Your cards, answers and reasons stay hidden until each reveal, and count like anyone's. Never tell anyone one of them before then: an early number anchors the team. People read your reason by hovering your seat, so keep it to a line or two, about the work.

## Tips

- Every tool takes a room's 5-character code or its link. Not given either? `list_rooms` lists the user's rooms, the most recently active first; confirm which one when it isn't obvious. Each room is also a resource, `estimay://rooms/<code>`, which the user can mention with `@`.
- `import_stories` says which links it could not add and why (not a tracker link, already in the room, the tracker not connected to the room); it adds the rest.
- Stories are named by id in `get_room`; `edit_story`, `start_voting` and `set_estimate` take that id, and `delete_story` a list of them (all or none are deleted).
- Disciplines (like FE, BE, QA) split a story into parts voted on separately: set the room's with `set_disciplines` (or `disciplines` on `create_room`), then give a story the ones it needs in `create_room`, `add_stories` or `edit_story`. Only split a story when the user wants separate estimates.
- Extra questions (`risk`, `clarity`, `agentReady`, `aiTokens`) are asked next to the estimate, each with its own cards: set them with `extra_questions` on `create_room` or with `set_extra_questions`, and only when the user wants them. `get_room` lists them under `questions`, with each story's agreed `answers`. To answer one for the user, call `vote` with its `question` and the card they named.
- Adding stories, voting rounds, settings and estimates need the host or a facilitator; `set_facilitator` (host only) takes the person's name as `get_room` lists it.
- A room someone else created needs `join_room` first (with its password, if it has one).
- `end_room` deletes the room for everyone. Export the results first, and only end it when the user asks.
- Errors come with what to do next ("Call join_room first."). Follow them rather than retrying blindly.
