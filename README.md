# Spinach plugin for Cursor

Give Cursor agents access to your meeting history. The plugin wires up the [Spinach](https://spinach.ai) MCP server and ships skills and a rule that teach the agent when and how to consult meeting context.

## What you get

- **Search past meetings** by keyword or intent (hybrid BM25 + semantic).
- **Fetch full meeting content** — summaries, action items, decisions, chapters, participants, transcripts.
- **List and filter meetings** by date, participant, platform, recurring series, or curated collection.
- **Read live captions** from in-progress meetings so the agent can act on the conversation.

The agent reaches for these capabilities automatically when the user references discussions, decisions, or commitments.

## Installation

Install via the Cursor marketplace (once published) or use the `/plugin` flow with this repository.

The first time you trigger a Spinach tool, Cursor opens a browser window where you sign into Spinach and click **Allow**. Spinach manages the OAuth flow itself — no client ID or secret required.

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

Exposes the Spinach remote MCP server at `https://mcp.spinach.ai/mcp`. The available tools are:

| Tool | Purpose |
|:-----|:--------|
| `list_meetings` | List/filter meetings with lightweight metadata (title, date, participants, summary snippet, action/decision counts) |
| `search_meetings` | Hybrid BM25 + semantic search across indexed meeting chapters; tunable via `vector_weight` |
| `get` | Fetch full meeting content (summary, action items, decisions, chapters, transcript) by ID |
| `list_collections` | List curated collections of meetings the user has access to |
| `get_live_meeting` | Read live captions from an in-progress meeting |

## Privacy

Meeting data flows through the user's authenticated Spinach session. Spinach is SOC2 Type II, GDPR, and HIPAA compliant; user data is never used for AI model training. See [Spinach security](https://spinach.ai/security) for details.

The bundled rule and skills explicitly warn the agent not to paste meeting content into commits, public PRs, or external chat.

## License

MIT
