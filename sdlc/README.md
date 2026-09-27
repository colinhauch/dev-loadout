# The loop — this repo's AI-native SDLC

How this repo takes a feature from idea to production. Tracker mechanics (the
`gh` commands) live in `docs/agents/issue-tracker.md`.

## The idea in one line

A feature is **a chain of linked GitHub issues** — `intent → spec → tickets` —
each stage a lossy compression of the last, so a cold session reads the current
issue instead of re-exploring.

Two things earn the ceremony:
1. **Resume across sessions.** A fresh session reads the issue cold under a token
   budget instead of re-deriving context.
2. **Audit trail.** Each stage keeps its own issue, body, and comments, and links
   to the one it came from — so what was asked, what got built, and why all stay
   recoverable.

## Where the SDLC lives

| Artifact | Home | Lifetime |
|---|---|---|
| Intent, spec, tickets | GitHub issue | outlive any branch |
| Plan, review findings | branch / PR body | die with the branch |
| CLAUDE.md, skills, hooks | `main` | steer every session |

**Issues are canonical.** The intent, the spec, and each ticket are issues, not
files.

## Right-size the chain

The chain is a tool, not a tax.

| Change | Do |
|---|---|
| Typo, dep bump, one-line fix | nothing — just commit |
| Small, well-understood fix | a ticket issue (or nothing) |
| A real feature | full chain: intent → spec → tickets |
| Bug fix | an issue with the repro → failing test → fix |

## The stages

Three label axes carry the state, all defined in `docs/agents/triage-labels.md`:
**category** (`bug` / `enhancement`), **state** (the five triage roles), and
**stage** (`intent` / `spec`, this chain's own). The state role is the gate —
`/triage` owns moving an issue through it, and an issue at `needs-triage` has not
been evaluated by anyone. An idea that does not survive is closed as `wontfix`.

Each stage opens a **new** issue linked to the one before, and closes its
predecessor. `Intent: #<n>` in a spec's body, `Spec: #<n>` in a ticket's — plain
references, not sub-issues, which GitHub renders as cross-references on the
issue they name.

### 1. Capture → `intent` issue
Run `/intent`. It brainstorms the raw idea with you, then opens a GitHub issue
labeled `intent`, its category, and `needs-triage`: problem, why,
roughly-better, out-of-scope, open questions. No design. Unspecified ideas
accumulate as a queue (`gh issue list --label intent`) and can be worked in any
order.

### 2. Triage → clear the intent
Run `/triage`. It evaluates the request against the codebase — is this already
built, was it rejected before, does the claim hold up — and moves the intent to
`ready-for-human` (cleared for a grilling session), `needs-info` (ask the
originator), or `wontfix` (closed). Only a `ready-for-human` intent is ready for
stage 3.

### 3. Spec → a new linked `spec` issue
A working session takes one cleared intent, grills it (`/grill-me`,
`/grill-with-docs`, constrained by `CONTEXT.md` and the `domain-modeling` /
`codebase-design` skills), then `/to-spec` publishes a **new** issue labeled
`spec` and `ready-for-agent`, carrying `Intent: #<n>` in its body. Close the
intent. The link is the drift guard: a reviewer checking whether the spec still
answers what was asked opens the intent rather than trusting a paraphrase.
Resolve flagged concerns before anything gets cut.

### 4. Tickets → child issues
`/to-tickets` cuts the spec issue into issues at `ready-for-agent`, each sized to
one session, with native blocking edges and `Spec: #<n>` in the body. **Each
ticket must be self-sufficient** — a ticket that needs its parent spec to be
actionable is defective. A ticket carries no stage label, which is what
distinguishes it from the spec it came from.

### 5. Build → plan + code
Start in **plan mode**, hand Claude the ticket, iterate until an unseen session
could build from the plan alone. The plan lives in the branch/PR, not an issue.
Then `/implement` (test-driven). If implementation departs from the plan, keep the
plan honest in the same commit. Use worktrees for parallel features.

### 6. Test → tests + CI
Every session verifies its own work before you see it. Bug fixes are **test-first**
(`/tdd`): write the failing test, commit it, then make it pass without editing the
test. CI gates every merge. "Done" means the tests are green.

### 7. Review → PR
Run `/code-review` against the **Review policy**
below; findings go in the PR, not a file. Open a PR into `main`; a maintainer does
the final light review before merge.

## Review policy

One pass covering bugs plus spec compliance (the `code-review` skill runs these):

- **Bugs** — logic errors, broken edge cases, regressions.
- **Security** — injection, auth gaps, secrets in code or docs, and any client
  information (see `README.md`).
- **Spec compliance** — does the diff do what the spec issue required?
- **Standards** — repo conventions (`CLAUDE.md`, `CONTEXT.md`).

**Important vs Nit:** reserve *Important* for anything that breaks behavior, leaks
data, or breaks a policy. Style and naming are nits — cap at five per review,
summarize the rest as a count. Skip generated files and anything CI already
enforces.

## What we deliberately skip

Enterprise controls exist to govern a large org and are overhead at this size:
PRD committees, sign-offs, separation-of-duties approval hooks, MDM/managed
settings, OpenTelemetry export, DORA dashboards. Ignored on purpose.
