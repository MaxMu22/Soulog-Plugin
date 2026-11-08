---
name: ask-my-logs
description: Answer questions across the user's Soulog Logs, Spaces and to-dos, or prepare a brief before a meeting, with every claim traced to its source recording. Use when the user asks what was said, decided or promised across past recordings, about a person, client, project or topic in Soulog, or wants context before a meeting — e.g. "what concerns has Acme raised so far", "prep me for my call with Tom tomorrow", "what did I promise the design team this month", "我和这个客户前后聊过哪些顾虑", "明天见 Tom 前帮我准备一下".
---

# Ask across Soulog Logs

The user's Soulog Logs are their personal context. Answer from that context, show where each point came from, and say plainly when the Logs don't cover something.

## 1. Find the relevant Logs

- **By meaning** (a topic, a concern, a person): Soulog `search` with a focused natural-language query, `limit` 10–20. Run 2–3 differently worded queries if the first one is thin.
- **By time** ("this month", "since the kickoff"): `list_assets` with `from`/`to`.
- **Scoped to a client or project**: `list_projects` to find the matching Space, then pass its id as `project_id` to `search`. `get_space_summary` (or `get_project_summary` for older projects) gives the Space-level picture.
- **Commitments**: `list_todos` with `status: open` (optionally per `asset_id`).

If the person or project name is ambiguous, ask once which one they mean.

## 2. Verify before you answer

Search snippets are leads, not evidence. For each point you plan to make:

1. Read the Log's `get_summary` to understand the context.
2. Confirm the specific point with `grep_transcript` (a regex on the key words or name) to get the exact wording and `[HH:MM:SS]` timestamp.

Don't read whole transcripts unless the question truly needs it.

## 3. Answer

Reply in the user's language. Lead with the direct answer, then the supporting points:

```
<Direct answer in 1–3 sentences.>

- <point> — <Log title>, <date> [00:14:20]
- <point> — <Log title>, <date> [00:03:05]

Not covered in your Logs: <what you looked for but didn't find, if relevant>
```

- When points from different dates conflict, show both, newest first, and say what changed.
- Label your own conclusions as "(my read)" so they stay separate from what was said.

### Meeting prep variant

When the user is preparing for a meeting with a person or client, use this shape instead:

```
## Brief: <person / client> — <meeting date if known>
**Where things stand:** 2–3 sentences.
**History:** dated bullets of past conversations, newest first, each cited.
**Open commitments:** from list_todos and from action items in past Logs — who owes what.
**Unresolved concerns or questions:** each cited.
**Suggested agenda:** 3–5 items (my read).
```

## Never

- This skill is read-only. Do not create, edit or delete anything in Soulog.
- Don't present search snippets you haven't verified as facts.
- If a Soulog tool returns an error with `structuredContent.error`, show `error.message` to the user as is. Do not retry or work around it.
