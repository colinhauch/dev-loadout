---
name: setup-sdlc
description: "One-time setup for a repo created from this template: ask which agents and issue tracker the project uses, prune tooling it doesn't need (e.g. OpenAI/Codex agent metadata), configure the tracker and SDLC labels, and clear template leftovers. Run once in a new repo."
disable-model-invocation: true
---

# /setup-sdlc: initialise a new repo from this template

Run once, right after creating a repo from this template. It is a prompt-driven skill, not a script: explore, present what you found, confirm, then write. Ask one question at a time and lead with the recommended answer so it can be accepted in a word.

This skill owns only what is specific to this SDLC (`sdlc/README.md`). Tracker, triage-label and domain-doc configuration belongs to `/setup-matt-pocock-skills`; hand off to it rather than duplicating it.

## Process

### 1. Explore

Read, don't assume: `git remote -v`, `docs/agents/`, `CLAUDE.md`, `README.md`, `LICENSE`, `skills-lock.json`, `.agents/skills/`, `.claude/skills/`, and `GLOSSARY.md`. Note what is still template placeholder versus already customised, so a re-run never clobbers real edits.

### 2. Agents and skills

Custom skills live in `sdlc/skills/<name>/` (the source of truth, symlinked into `.claude/skills/`). Upstream skills come from skills.sh and are listed in `skills-lock.json`; they are installed as **real copies** so each agent gets plain files, with no `.agents/` store or symlinks.

Ask which coding agents this project uses: Claude Code only (recommended), or also Codex, Gemini CLI, Copilot, etc. Then ask whether to keep every upstream skill in `skills-lock.json` (default) or a trimmed list; remove the unwanted entries from the lockfile and `.claude/skills/`.

Install the upstream skills for the chosen agents (zsh-safe; pass the names as separate arguments):

```
npx skills add mattpocock/skills -a <agent> --copy -s <skill> <skill> ... -y
```

- Claude Code: `-a claude-code` writes `.claude/skills/<name>/`.
- Codex and other agents: add them to `-a` (or re-run later with `-a codex`); they read `.agents/skills/`, so that directory appears only when such an agent is chosen.
- Do not use `npx skills experimental_install`: it always restores into `.agents/skills/` and ignores `--copy` and `-a`.
- `npx skills update -p -y` updates copies in place afterwards.

Claude Code never reads `agents/openai.yaml`. Unless Codex was chosen, delete every `agents/` folder that contains only `openai.yaml` under `.claude/skills/` (and `.agents/skills/` if present). Installs and updates restore them, so re-run this step after either.

### 3. Issue tracker, labels, domain docs

Run `/setup-matt-pocock-skills` to configure the tracker, triage labels and domain docs (`docs/agents/`). Do not re-ask what it asks.

### 4. SDLC labels

The feature chain uses two labels beside the triage roles: `intent` and `spec` (`docs/agents/triage-labels.md`). If the tracker is GitHub/GitLab, offer to create them (`gh label create intent`, `gh label create spec`), skipping any that exist. For a local tracker, nothing to create.

### 5. Template leftovers

List, then ask before changing each:

- `README.md` and `LICENSE`: still describe or license the template? Offer to replace with a stub for the new project.
- `CLAUDE.md`: references to the template's own name or repo.
- `sdlc/ideas.md`: clear any example entries.
- `skills-lock.json` is real state; leave it.

### 6. Report

Summarise what was removed, created and left alone, and point to `/intent` as the first thing to run. Do not commit unless asked.
