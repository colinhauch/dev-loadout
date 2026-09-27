# Triage Labels

The skills speak in terms of canonical triage roles. This file maps those roles
to the actual label strings used in this repo's issue tracker.

## Category roles

| Canonical     | Our tracker   | Meaning                  |
| ------------- | ------------- | ------------------------ |
| `bug`         | `bug`         | Something is broken      |
| `enhancement` | `enhancement` | New feature or improvement |

## State roles

| Canonical         | Our tracker       | Meaning                                  |
| ----------------- | ----------------- | ---------------------------------------- |
| `needs-triage`    | `needs-triage`    | Maintainer needs to evaluate this issue  |
| `needs-info`      | `needs-info`      | Waiting on reporter for more information |
| `ready-for-agent` | `ready-for-agent` | Fully specified, ready for an AFK agent  |
| `ready-for-human` | `ready-for-human` | Requires human implementation            |
| `wontfix`         | `wontfix`         | Will not be actioned                     |

The strings are identical to the canonical names, so a skill can use either.
When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the
label of the same name.

## SDLC stage labels

Two further labels sit *alongside* the roles above rather than replacing them,
and belong to this repo's feature chain (`sdlc/README.md`):

- `intent` — a captured idea, pre-spec.
- `spec` — requirements and design, ready to cut into tickets.

A triaged issue carries one category role and one state role; an issue in the
feature chain also carries one stage label. The stage label is what
disambiguates a `spec` at `ready-for-agent` (cut it into tickets) from a ticket
at `ready-for-agent` (build it).
