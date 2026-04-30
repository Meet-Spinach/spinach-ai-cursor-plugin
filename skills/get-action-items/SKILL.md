---
name: get-action-items
description: Surface action items assigned to the user or related to a topic from recent Spinach meetings. Use when the user asks "what did I commit to", "what are my todos", or wants to follow up on a meeting.
---

# Get action items

## Trigger

The user wants to know what they (or someone else) committed to in recent meetings, or wants to act on outstanding follow-ups.

## Workflow

1. Establish scope:
   - Time window (default: last 7 days). Translate to `from_date` / `to_date` in ISO 8601.
   - Person (default: the current user). Use `participant` filter on `list_meetings`.
   - Topic, if specified — use `search_meetings` instead of `list_meetings`.
2. Use `list_meetings` with `fields: ["id", "title", "date", "participants", "action_item_count"]` and the date/participant filters. Skip meetings where `action_item_count` is 0.
3. For each remaining meeting, call `get` with `include: ["action_items", "decisions"]`.
4. Aggregate the action items, attribute by owner where the meeting summary makes ownership clear, and de-duplicate items that appear in multiple meetings.
5. Sort by recency unless the user asks otherwise.

## Guardrails

- Don't invent owners. If an action item lacks a clear owner in the summary, say "owner not specified".
- Meeting summaries are AI-generated — flag items that look ambiguous so the user can verify before acting.
- If the user asks to act on an item (open a PR, file a ticket, draft an email), confirm the item with them before taking irreversible action.

## Output

- Grouped list of action items by meeting, with date and meeting URL.
- Optional: a "for you" filtered view if a specific person was requested.
- Note about any items that look ambiguous and need human confirmation.
