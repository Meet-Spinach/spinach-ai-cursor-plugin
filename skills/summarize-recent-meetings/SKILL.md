---
name: summarize-recent-meetings
description: Generate a concise recap of recent Spinach meetings — decisions, action items, and themes — for a given time window or recurring series. Use when the user asks for a daily/weekly recap or "what did I miss".
---

# Summarize recent meetings

## Trigger

The user wants a recap. Common phrasings:

- "What happened in standup yesterday?"
- "Summarize this week's meetings."
- "What did I miss while I was out?"

## Workflow

1. Resolve the window:
   - "Today" / "yesterday" → single-day `from_date`/`to_date`.
   - "This week" → Monday 00:00 → now.
   - "While I was out" → ask the user for the date range if it isn't already clear.
2. Resolve the scope:
   - Specific recurring meeting → `list_meetings` with `recurring_meeting_series_id`.
   - All meetings the user attended → `list_meetings` with `participant: "<user>"`.
   - A curated group → `list_collections` then `list_meetings` with `collection_id`.
3. Call `list_meetings` with `fields: ["id", "title", "date", "tldr", "summary_snippet", "action_item_count", "decision_count"]` and `limit: 100`. The `tldr` and `summary_snippet` are usually enough for a recap.
4. Only call `get` for a meeting if its snippet is empty or the user wants depth on a specific one.
5. Group output by day or by topic — pick whichever yields fewer redundant bullets.

## Guardrails

- Recaps should be skimmable. Aim for ≤2 lines per meeting.
- Roll up duplicate decisions across recurring meetings into a single bullet.
- Preserve attribution when discussing decisions or action items ("Lucas to investigate X").
- If `tldr` and `summary_snippet` are both empty for several meetings, mention that the recap is incomplete rather than fabricating content.

## Output

- Date-range header.
- Per-meeting bullets (title, date, 1–2 line gist) with the meeting URL.
- Cross-cutting "Decisions" and "Action items" sections at the end.
