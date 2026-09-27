# Template

## The SDLC

Features move `intent → spec → tickets → code+tests → review`, carried as GitHub
issues. The playbook is [sdlc/README.md](sdlc/README.md); the working notes and
skill inventory are in [sdlc/CLAUDE.md](sdlc/CLAUDE.md). Start a feature or resume
one cold with `/sdlc`; capture a raw idea with `/intent`.

## Agent skills

### Issue tracker

Issues, specs, and tickets live as GitHub issues in this repo, via the `gh`
CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Canonical triage roles, plus `intent` and `spec` stage labels.
See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: `CONTEXT.md` and `docs/adr/` at the repo root, created lazily by
`/domain-modeling`. See `docs/agents/domain.md`.
