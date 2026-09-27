---
name: sdlc
description: "Navigate this repo's feature SDLC lifecycle (intent → spec → tickets) as tracker issues. Use when starting a feature, resuming one across a session boundary, or when asked where a feature lives or what stage it's at."
---

# SDLC — the feature lifecycle

The chain is `intent → spec → tickets → code+tests → review`. Each stage is its
own issue, linked to the one before. Where issues live, and every command that
touches them, is `docs/agents/issue-tracker.md` — read it; this skill does not
restate it. The label vocabulary is `docs/agents/triage-labels.md`.

Artifact homes by lifetime:

| Artifact | Home |
|---|---|
| Intent, spec, tickets (outlive the branch) | the issue tracker |
| Plan, review findings (die with the branch) | branch / PR body |
| CLAUDE.md, skills, hooks (steer every session) | the repo's default branch |

## Advancing a stage

| From | Do | Produces |
|---|---|---|
| raw idea | `/intent` | `intent` issue at `needs-triage` |
| `intent` at `needs-triage` | `/triage` | the intent at `ready-for-human`, or closed |
| `intent` at `ready-for-human` | `/grill-me` / `/grill-with-docs`, then `/to-spec` | a new `spec` issue at `ready-for-agent`, linked back |
| `spec` issue | `/to-tickets` | child issues at `ready-for-agent` |
| ticket | plan mode, then `/implement` / `/tdd` | code + tests |
| code done | `/code-review` | review in the PR |

Tickets must be self-sufficient: a ticket that needs its parent spec to be
actionable is defective.

## Stages are linked issues

Each stage publishes a **new** issue rather than relabeling the last one, so
every stage keeps its own body, comments, and edit history. Carry the chain in
the new issue's body as a plain reference — `Intent: #<n>` on a spec, `Spec: #<n>`
on a ticket — which GitHub renders as a cross-reference on the issue it names.
These are references, not sub-issues: a spec is the successor of its intent, not
a child of it.

Close the predecessor once its successor exists. The intent closes when its spec
issue opens; the link preserves the trail, and the open `intent` queue stays a
list of ideas that have not yet been specified.

Because the intent is no longer inlined at the top of the spec, the reference is
the **drift guard**: a reviewer checking whether a spec still answers what was
asked opens the linked intent. Never paraphrase the intent into the spec in place
of the link.

## Labels

Three axes, all from `docs/agents/triage-labels.md`:

- **Category**: `bug` or `enhancement`.
- **State**: the five triage roles. This is the gate — an issue at
  `needs-triage` has not been evaluated, and `/triage` owns moving it.
- **Stage**: `intent` or `spec`, this chain's own. A ticket carries no stage
  label, which is what distinguishes a `spec` at `ready-for-agent` (cut it) from
  a ticket at `ready-for-agent` (build it).

Grilling an intent into a spec reads this repo's domain docs first —
`docs/agents/domain.md` says which ones and how.

## Resuming across a session boundary

To pick up a feature cold, fetch the relevant issue and the one it links to. Its
labels say the stage and the state; its body and comments say what's left. A
read, not a re-exploration.

Find work by listing on the pair:

- Unspecified ideas: `--label intent`
- Intents cleared to grill: `--label intent --label ready-for-human`
- Specs ready to cut: `--label spec --label ready-for-agent`
- Grabbable tickets: `--label ready-for-agent` minus `--label spec`

`sdlc/` holds the process docs and the skill sources. Feature artifacts don't
live there — the issue is the record.
