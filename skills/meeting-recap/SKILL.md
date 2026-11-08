---
name: meeting-recap
description: Recap a meeting or conversation recorded in Soulog into decisions, action items (owner, due date) and open questions, each cited to its timestamp in the recording. Use when the user asks to recap, summarize, review or "write up" a Soulog recording, meeting, call, interview or Log — e.g. "recap my call with Acme this afternoon", "what did we decide in yesterday's standup", "整理一下今天和客户的会", "这次会议的待办有哪些".
---

# Meeting recap from Soulog

Turn one Soulog Log (a recording with its transcript, summary and any attached notes, images or files) into a recap the user can act on. Every decision and action item must point back to where it was said.

## 1. Find the right Log

- If the user gives a time ("this afternoon", "last Tuesday"), use the Soulog `list_assets` tool with `from`/`to` dates (yyyy-MM-dd, interpreted in the user's local date) and `type: AUDIO`. Add `keyword` if they mention a title word.
- If they describe the topic or a person instead ("the pricing call with Tom"), use the Soulog `search` tool.
- If the user says "my last meeting", use `list_assets` with `sort: CREATED_AT`, `order: desc`, `limit: 5`.
- **One clear match** → continue. **Several plausible matches** → list up to 3 with title, date and duration and ask which one. **No match** → say so and show what you searched for; do not recap a different recording.

## 2. Read only what you need

1. `get_asset` — title, time, duration, speakers, summary status.
2. `get_summary` — the latest Markdown summary is the best starting point. If there is none, fall back to `get_transcript` (`format: text`; follow `next_cursor` if `truncated` is true).
3. `grep_transcript` — for each decision and action item, find the supporting line to get its exact `[HH:MM:SS]` timestamp and wording. Prefer this over reading the whole transcript.
4. If the user's request depends on attached notes or files, read them too and cite them by name.

## 3. Write the recap

Reply in the user's language. Keep quotes in the language they were spoken.

```
## <Log title> — <date>, <duration>
Participants: <speakers as labelled in the transcript>

**In short:** 2–3 sentences on what the meeting was about and where it landed.

### Decisions
- <decision> — [00:12:34]

### Action items
| Action | Owner | Due | Source |
|---|---|---|---|
| <verb-first task> | <name or "Not stated"> | <date or "Not stated"> | [00:23:10] |

### Open questions
- <unresolved question or disagreement> — [00:31:05]

### Key quotes (optional, max 3)
> "<exact words>" — <speaker>, [00:40:12]
```

Rules:
- **Never invent owners or due dates.** If the recording does not say, write "Not stated". If you infer something, label it "(inferred)".
- Keep what was *said* separate from your own interpretation.
- Skip empty sections rather than padding them.
- Cite timestamps from the transcript, not from the summary text.

## 4. Offer the next step — don't take it

End with one short line offering what usually comes next, for example: "Want me to send this to your docs or task tool, or save it back to Soulog?" Use the `send-to-workflow` or `save-to-soulog` skills only if the user says yes.

## Never

- Never call `write_summary`, `update_note`, `write_transcript` or any other Soulog write tool from this skill. Recapping is read-only.
- If a Soulog tool returns an error with `structuredContent.error`, show `error.message` to the user as is. Do not retry or work around it (this includes plan/entitlement errors).
