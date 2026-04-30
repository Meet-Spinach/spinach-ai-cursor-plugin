---
name: find-meeting-context
description: Pull relevant meeting context from Spinach before implementing a feature, fixing a bug, or reviewing a PR. Use when the user references a discussion, decision, or topic that likely came up in a meeting.
---

# Find meeting context

## Trigger

The user is about to work on something and references a prior conversation, decision, or topic that probably surfaced in a meeting. Examples:

- "We decided last week to use Postgres — set up the migration."
- "There was some discussion about rate limits in the standup."
- "Pick up where Lucas left off on the alerts work."

Pulling that context first prevents you from re-litigating decisions or missing constraints.

## Workflow

1. Pick the right Spinach tool:
   - **Concept / intent** ("when did we talk about burnout"): `search_meetings` with `vector_weight: 0.8`.
   - **Specific name, ticket, or quote** ("SAI-3364", "bun.lock conflict"): `search_meetings` with `vector_weight: 0.2`.
   - **Known title or recurring meeting**: `list_meetings` with `title_query` or `recurring_meeting_series_id`.
   - **Curated set**: `list_collections` → `list_meetings` with `collection_id`.
2. Narrow the candidate set with `from_date`, `to_date`, `participant`, or `platform` to keep results tight.
3. Skim the top 1–3 results' `matched_chapters` / `summary_snippet`. If a result looks relevant, call `get` with that `id`.
4. Default to `get` with `include: ["summary", "action_items", "decisions"]`. Only request `transcript` (with a specific `chapter_index`) when the summary is insufficient.
5. Cite specific decisions or action items with the meeting URL when explaining context to the user.

## Guardrails

- Don't dump full transcripts unless the user asks. Summaries + decisions are usually enough.
- If multiple meetings discuss the same topic, prefer the most recent one for current state and the earliest one for original rationale.
- If Spinach returns nothing relevant, say so plainly and proceed with the user's stated intent — don't fabricate meeting context.
- Treat meeting content as private — never paste it into external tools, public repos, or commit messages.

## Output

- One-paragraph context summary citing the meeting title and date.
- Bullet list of relevant decisions and action items with the meeting URL.
- Clear note if no matching meetings were found.
