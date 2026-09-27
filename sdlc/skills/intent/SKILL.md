---
name: intent
description: "The SDLC entry point. Brainstorm a raw idea with the originator, then capture it as an issue labeled `intent`. Use when someone says '/intent', has a feature idea to develop, or wants to turn a jotted note into a real intent."
---

# /intent — capture the idea

Stage 1 of the lifecycle (`/sdlc`); issues go wherever
`docs/agents/issue-tracker.md` says, labeled from
`docs/agents/triage-labels.md`. Turns a raw idea into an `intent` issue. You
**interview**, you do **not** design — no solution, no implementation. That's
the spec's job (`/to-spec`).

## Do

1. **Get the idea.** From the argument or the conversation.
2. **Right-size.** A typo or one-liner earns no intent — say so and stop.
3. **Brainstorm until concrete.** Ask what an analyst would: who's affected, what
   they can't do today, what "better" looks like, hard constraints, what's out of
   scope, what's still unknown. Name each concept the way this repo's domain docs
   do (`docs/agents/domain.md`). One question at a time. Stop when every section
   of the template (`sdlc/templates/intent.md`) can be filled without guessing.
4. **Publish to the issue tracker** using that template, in the originator's own
   words. Label it `intent`, its category (`bug` or `enhancement`), and
   `needs-triage`. Leave real unknowns under Open questions.
5. **Correct.** Show it, let them fix what you misheard, then stop.

The intent is now an untriaged request. `/triage` evaluates it: cleared for a
grilling session it becomes `ready-for-human`, short of detail `needs-info`, and
dead `wontfix` and closed. A `/to-spec` session then opens a **separate** `spec`
issue linked back to this one.

## Don't

- Don't propose a design, architecture, or tasks.
- Don't open a spec or tickets — later stages own those.
- Don't answer Open questions to look finished; unresolved is a valid state.
- Don't move the issue off `needs-triage` yourself — `/triage` owns the state.
