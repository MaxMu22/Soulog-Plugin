---
name: save-to-soulog
description: Save something from this Claude conversation back into Soulog as a new Log — an analysis, plan, draft or notes Claude produced, or text the user provides — linked to the recording it came from and filed into a Space if asked. Use when the user says to save, store, keep or log something "in Soulog" — e.g. "save this analysis to Soulog", "add this to my Acme space in Soulog", "attach these notes to yesterday's recording", "把这份分析存回 Soulog", "存成一条新的 Log".
---

# Save to Soulog

Soulog is where the user's context lives. When work done in Claude should become part of that context, save it as a new Log, with its sources, without touching what is already there.

## 1. Decide what and where

**Content:** the exact text the user wants saved (usually Claude's latest analysis or recap). Save it as Markdown. Don't summarize or shorten it unless the user asks.

**Where it goes** (pick one; ask only if it's genuinely unclear):

| Situation | Tool | Result |
|---|---|---|
| A new, standalone piece of work (default) | `create_note` without `asset_id` | A new Log in Soulog |
| Notes that belong to one specific recording ("attach to yesterday's call") | `create_note` with that `asset_id` | A note attached to that recording |

**Space (optional):** if the user names a client, project or Space, resolve it with `list_projects`. After the note is created, add it with `add_assets_to_space` (`space_id` + the new note's id). If no Space matches, say so and save without one rather than guessing.

## 2. Add provenance

Append a short footer so the Log stays traceable in Soulog:

```
---
Created with Claude on <date>.
Based on: <Log title> (<date>)[, <Log title> (<date>)...]
```

List only Logs you actually read in this conversation. Omit "Based on" if the content didn't come from Soulog.

## 3. Confirm, then save once

Show a preview and wait for the user to confirm:

```
Save to Soulog:
- Title: <title, ≤100 characters>
- As: new Log | note on "<recording title>"
- Space: <space name or none>
- Content: <first 2–3 lines>… (<N> characters)
Save?
```

Then call the tool **once**. Limits: title 1–100 characters; `create_note` content ≤ 100,000 characters. If the content is longer, ask whether to split it into parts or trim it.

If a call times out or the result is unclear, check with `list_assets` (`type: NOTE`, `keyword` = title) before trying again, so the user doesn't get duplicates.

## 4. Report back

Confirm the title, where it was saved (and which Space), and the new id, so the user can find it in the Soulog app.

## Files

Prefer Markdown notes. Only use `upload_artifact` for a real file (PDF, PPT, document) when the user explicitly asks *and* this environment can make the HTTP `PUT` to the returned `upload_url`. If it can't, tell the user to add the file in the Soulog app instead.

## Never

- Never overwrite existing content. Don't use `update_note`, `write_summary` or `write_transcript` unless the user explicitly asks to edit a specific existing note or summary, and then show exactly what will change first.
- Never save without the user's confirmation.
- If a Soulog tool returns an error with `structuredContent.error`, show `error.message` to the user as is. Do not retry or work around it.
