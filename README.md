# Soulog for Claude

**Log your life. Put your context to work.**

[Soulog](https://www.soulog.com/) captures what happens — recordings, transcripts, summaries, and the photos, files and notes you add along the way — and organizes it into structured Logs. This plugin brings that context into Claude so it flows straight into the way you already work, and lets you save what you create in Claude back to Soulog.

## What you can do

| Skill | Try saying | What happens |
|---|---|---|
| **meeting-recap** | "Recap my call with Acme this afternoon." | Finds the Log and returns decisions, action items (owner, due date) and open questions, each cited to its timestamp in the recording. |
| **ask-my-logs** | "What concerns has Acme raised so far?" / "Prep me for my meeting with Tom tomorrow." | Searches across your Logs, Spaces and to-dos and answers with sources, or builds a pre-meeting brief. |
| **send-to-workflow** | "Put this recap in Notion and create the action items in Asana." | Hands the recap to the docs and task tools you've connected in Claude (Notion, Google Docs, Lark/Feishu, Asana, Linear, Jira, Todoist and others). If none are connected, you get clean Markdown and a checklist instead. |
| **save-to-soulog** | "Save this analysis to my Acme space in Soulog." | Saves Claude's work as a new Log, linked to the recordings it came from, and files it into a Space. |

Skills trigger automatically from what you ask. You can also run them directly as `/soulog:meeting-recap`, `/soulog:ask-my-logs`, `/soulog:send-to-workflow` and `/soulog:save-to-soulog`.

## How it behaves

- **Sourced:** answers point back to the Log and timestamp they came from, and Claude's own interpretation is labelled separately.
- **Asks before writing:** nothing is written to Soulog or to any other tool until you've seen a preview and confirmed it.
- **Leaves your originals alone:** your recordings, transcripts and summaries are never changed or deleted. Saving always creates a new Log or note.
- **No extra integrations:** it works with whatever docs and task tools you already have connected in Claude, and never asks for passwords or API keys.

## Requirements

- A Soulog account. Access to the Soulog connector depends on your Soulog plan. See the [MCP guide](https://web.prod-us.soulog.com/mcp-guide).
- The plugin connects to the Soulog MCP server at `https://api.prod-us.soulog.com/mcp`. The first time a Soulog tool is used, you'll be asked to sign in to Soulog and authorize Claude (OAuth). You can revoke access at any time in the Soulog app.

## Install

From the Claude plugin directory, search for **Soulog** and install it.

Or install it in Claude Code directly from this repository:

```
/plugin install soulog --marketplace MaxMu22/Soulog-Plugin
```

For local testing with Claude Code:

```bash
claude --plugin-dir ./Soulog-Plugin
```

Then run `/mcp` and authenticate the `soulog` server.

## Data handling

- **What it reads:** your Soulog recordings' metadata, transcripts, summaries, notes, Spaces and to-dos, which may include names and other personal information, through the Soulog connector you authorize.
- **What it stores:** only when you confirm, `save-to-soulog` saves content from your Claude conversation to your Soulog account as a new note, and `send-to-workflow` can add to-dos to Soulog. Saved content stays in your Soulog account until you delete it.
- **Where else data goes:** only when you ask and confirm, `send-to-workflow` sends a recap or action items to other docs or task tools that **you** have connected in Claude (for example Notion, Google Docs, Asana or Linear). The plugin declares no other services, and it does not send data anywhere on its own.

## Privacy and support

- Privacy policy: https://web.prod-us.soulog.com/article/privacy-security
- Terms of service: https://web.prod-us.soulog.com/article/terms
- Support: https://www.soulog.com/pages/service-and-support

The plugin itself contains only instructions (skills) and the connector address. It runs no code of its own, and your Soulog data is accessed only through the Soulog connector you authorize.

## License

MIT © Soulog Inc.
