---
name: send-to-workflow
description: Hand a Soulog meeting recap, notes or action items to the tools the user already works in — a document in their docs tool (e.g. Notion, Google Docs, Confluence, Lark/Feishu Docs) and tasks in their task tool (e.g. Asana, Linear, Jira, Todoist, Lark/Feishu Tasks) — using whatever the user has connected in Claude, or clean Markdown when nothing is connected. Use when the user wants Soulog content turned into a doc or tasks — e.g. "put this recap in Notion", "create tasks for these action items", "sync the to-dos from my Soulog meeting", "把会议总结整理成文档", "待办同步到任务".
---

# Send Soulog content into the user's workflow

Soulog captures and organizes; this skill delivers the result where the user actually works. It does not bundle any third-party integration — it uses the tools the user has already connected in Claude.

## 1. Have the content ready

- If a recap or brief already exists in this conversation, use it.
- Otherwise, build one first with the `meeting-recap` skill (or `ask-my-logs` for multi-Log material).
- Keep the source line and timestamps; they travel with the content.

## 2. Pick the destination

1. **The user named a tool** ("in Notion", "to Asana") → use it if it is available in this session.
2. **Named tool not available** → say it isn't connected, suggest connecting it from Claude's connector settings, and offer the Markdown fallback (step 5). Don't pick a different tool silently.
3. **No tool named** → look at which document and task tools are available in this session. If exactly one of each kind fits, propose it. If several fit, ask which. If none, use the fallback.

Never ask the user for API keys, passwords or tokens. Only use connectors that are already available.

## 3. Show the plan, then wait for "yes"

Before writing anything, show a short preview:

```
I'll create:
- Doc in <tool> → "<title>" (in <location, if known>)
- <N> tasks in <tool> / <project or list>:
  1. <task> — owner: <name or unassigned> — due: <date or none>
  2. ...
Go ahead?
```

Proceed only after the user confirms. Apply any edits they ask for first.

## 4. Write

**Document**
- Title: `<Log title> — recap (<date>)` unless the user gave one.
- Body: the recap as-is (decisions, action items, open questions), plus a footer: `Source: Soulog — <Log title>, <date>. Timestamps refer to the original recording.`
- Use the destination's native formatting (headings, tables, checklists) where it supports them.

**Tasks** — one per action item:
- Title: verb-first, under ~80 characters.
- Description: one or two lines of context, plus `From Soulog: <Log title>, <date> [HH:MM:SS]`.
- Due date: only if one was stated in the recording. Never invent one.
- Assignee: only when the person clearly matches a user in the destination tool. Otherwise leave it unassigned and put the name in the description.

**Optional: keep Soulog in sync.** If the user asks, also create each action item in Soulog with `add_todo`, anchored to the source Log (`asset_id`).

## 5. Fallback when nothing is connected

Give the user:
- the document as clean Markdown they can paste anywhere, and
- the action items as a checklist (`- [ ] task — owner — due`).

If this environment can create files, offer a `.md` (or `.docx`) file as well.

## 6. Report back

List what was created, with links or ids returned by each tool. If some writes failed, say which ones and show the error. Don't retry a failed write automatically, since that can create duplicates.

## Never

- Never write to any tool before the user confirms the preview.
- Never change or delete the original Soulog Log, its summary or transcript.
- If a Soulog tool returns an error with `structuredContent.error`, show `error.message` to the user as is.
