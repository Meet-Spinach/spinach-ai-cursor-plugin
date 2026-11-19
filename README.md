# Spinach plugin for Cursor

Give Cursor agents access to your meeting history. The plugin wires up the [Spinach](https://spinach.ai) MCP server and ships skills and a rule that teach the agent when and how to consult meeting context.

## What you get

- **Search past meetings** by keyword or intent (hybrid BM25 + semantic).
- **Fetch full meeting content** — summaries, action items with owners, decisions, blockers, chapters, participants, transcripts, and a link to the recording.
- **List and filter meetings** by date, participant, platform, recurring series, or curated collection.
- **See what was on screen** — screenshots captured during a meeting, per chapter.
- **Read live captions** from in-progress meetings so the agent can act on the conversation.

The agent reaches for these capabilities automatically when the user references discussions, decisions, or commitments.

## Installation

1. Open **Cursor Settings → Plugins**.
2. Search for **Spinach**.
3. Click **Install**, then complete the Spinach sign-in prompt.

Or run `/add-plugin spinach` in chat. The same plugin works in Grok Bot.

Cursor opens a browser window where you sign into Spinach and click **Allow**. Spinach manages the OAuth flow itself — no client ID or secret required.

> Note: After first connection, it can take 5–10 minutes for your meeting data to be indexed.

## Components

### Skills (`skills/`)

| Skill | Description |
|:------|:------------|
| `find-meeting-context` | Pull relevant past meeting context before implementing a feature, fixing a bug, or reviewing a PR |
| `get-action-items` | Surface action items assigned to the user or related to a topic |
| `summarize-recent-meetings` | Generate a daily/weekly recap of decisions, action items, and themes |
| `follow-live-meeting` | Read live captions from an in-progress meeting and act on the conversation |

### Rules (`rules/`)

| Rule | Description |
|:-----|:------------|
| `consult-meeting-context` | Cue the agent to consult Spinach when the user references prior discussions, decisions, or commitments |

### MCP server (`mcp.json`)

Connects to the Spinach remote MCP server at `https://mcp.spinach.ai/mcp`:

```json
{
  "mcpServers": {
    "spinach": {
      "type": "http",
      "url": "https://mcp.spinach.ai/mcp",
      "placement": "server"
    }
  }
}
```

`"placement": "server"` makes Grok Bot connect through its hosted MCP path, which is what shows the Spinach sign-in card there.

The tools are:

| Tool | Purpose |
|:-----|:--------|
| `list_meetings` | List/filter meetings with lightweight metadata (title, date, participants, summary snippet, action item / decision / blocker / screenshot counts) |
| `search_meetings` | Hybrid BM25 + semantic search across indexed meeting chapters; tunable via `vector_weight` |
| `get` | Fetch full meeting content by ID: summary, action items, decisions, blockers, chapters, participants, transcript, collections, and a video link for recent recordings |
| `get_screenshots` | Fetch screenshots captured during a meeting, or an index of the available frames |
| `list_collections` | List curated collections of meetings the user has access to |
| `get_live_meeting` | Read live captions from an in-progress meeting |

The server is the source of truth for tool names and schemas. Some tools are enabled per account, so the list your client sees can differ.

## Privacy

Meeting data flows through the user's authenticated Spinach session. Spinach is SOC 2 Type II and GDPR compliant; user data is never used for AI model training. See [Spinach security](https://spinach.ai/security) for details.

Meeting transcripts and summaries include what other participants said. Treat them as confidential.

The bundled rule and skills explicitly warn the agent not to paste meeting content into commits, public PRs, or external chat.

## License

MIT
