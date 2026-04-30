---
name: follow-live-meeting
description: Read live captions from an in-progress meeting via Spinach so the agent can act on what was just said. Use when the user references "the meeting I'm in" or asks the agent to capture / react to a live discussion.
---

# Follow a live meeting

## Trigger

The user is currently in a meeting and wants the agent to act on what was just said — capture an action item, draft code based on a live decision, or summarize the discussion so far.

## Workflow

1. Call `get_live_meeting` with no `id` first.
   - If exactly one meeting is active, the response is the transcript.
   - If multiple are active, the response is `{ active_meetings: [...] }`. Ask the user which one, then call `get_live_meeting` again with that `id`.
   - If none are active, tell the user — Spinach has nothing to read.
2. Treat the transcript as `Speaker: words` lines. Recent lines at the bottom are most relevant.
3. Re-fetch periodically if the user wants ongoing context — there is no streaming; each call returns a snapshot.
4. Once the meeting ends, switch to `get` (not `get_live_meeting`) to read the post-processed summary, action items, and decisions.

## Guardrails

- Live captions require live captions to be enabled in the meeting platform and are only reliable in English. Surface this caveat if transcript quality is poor.
- Live transcripts can lag the actual conversation by several seconds. Don't claim something was "just said" — say "as of the latest captions".
- Never quote a participant verbatim outside the chat unless the user explicitly asks.
- If the user asks the agent to act on a live decision (push code, send a message), confirm before doing anything irreversible.

## Output

- Source line: meeting title + platform + how recent the captions are.
- A focused answer to the user's request grounded in the transcript.
- Note when the captions look incomplete or the meeting just started.
