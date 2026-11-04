# Working in `sdlc/`

This folder is the home of the repo's AI-native SDLC: the process docs and the
skills that drive it. `README.md` is the playbook (the operational how-to); this
file is the working notes — the *why* and the rules. Read the playbook for detail.

## The SDLC in a few lines

A feature moves `intent → spec → tickets → code+tests → review` as a chain of
linked **GitHub issues**: each stage opens a new issue, references the one before
in its body, and closes its predecessor. `/intent` captures an idea at
`needs-triage`, `/triage` clears it, `/to-spec` publishes the spec as a new
linked issue, `/to-tickets` cuts `ready-for-agent` child issues, then
`/implement` or `/tdd` builds. The label vocabulary is
`docs/agents/triage-labels.md`. Full stage-by-stage detail, right-sizing, and the
review policy are in `README.md`.

## Source of truth: GitHub issues

**Issues are canonical**, not files. The intent, the spec, and each ticket are
separate issues linked by a body reference (`Intent: #<n>`, `Spec: #<n>`). That
reference is the drift guard: never paraphrase a predecessor in place of the
link. The tracker mechanics — how to create/label/query, and how each
stage maps to `gh` — live in `docs/agents/issue-tracker.md`, the single place
that resolves "publish to the issue tracker." Skills read that indirection.

An artifact's home is its lifetime:

| Artifact | Home |
|---|---|
| Intent, spec, tickets (outlive the branch) | GitHub issue |
| Plan, review findings (die with the branch) | branch / PR body |
| CLAUDE.md, skills, hooks (steer every session) | `main` |

**Ticket self-sufficiency:** a ticket that needs its parent spec to be actionable
is defective. Each stage is a lossy compression of the one before — that caps
re-read spend on a cold start.

## The skills

Skill sources live under `sdlc/skills/<name>/`, each symlinked into
`.claude/skills/<name>`, so the concern stays one portable tree. Folder name =
skill name; add a skill by adding a folder and a symlink. Editing here updates the
live skill.

- `sdlc/skills/sdlc/` → `sdlc` — navigates the issue lifecycle (resume cold).
- `sdlc/skills/intent/` → `intent` — Stage 1 front door: `/intent` interviews a
  raw idea and opens an `intent` issue at `needs-triage`.
- `sdlc/skills/commit/` → `commit` — conventional-commit helper.
- `sdlc/skills/setup-sdlc/` → `setup-sdlc` — one-time setup for a repo made from
  this template: picks agents and tracker, prunes unused tooling, hands tracker
  config to `/setup-matt-pocock-skills`.

The rest of `.claude/skills/` comes from [skills.sh](https://skills.sh), pinned in
`skills-lock.json` at the repo root and installed as real copies
(`npx skills add <source> -a claude-code --copy -s <skills>`), so there is no
`.agents/` store. `/setup-sdlc` handles installing for the agents a project uses.

## Ideas inbox (pre-intent)

`sdlc/ideas.md` is a scratch list for half-formed ideas — cheaper than opening an
issue for something that isn't a commitment yet. `/intent` promotes a ripe entry
into an `intent` issue; the record starts there, not in `ideas.md`.

## Templates

`sdlc/templates/` holds the issue-body shapes each stage writes: `intent.md`,
`spec.md`, `plan.md`, `review.md`. `/intent` fills `intent.md`.

## Common tools

`sdlc/common-tools.md` lists project-agnostic tools (install commands, basic
usage) that aren't skills. Check it before recommending a tool.
