# Project Workflow

## ChatGPT project

Use the ChatGPT Project as the long-lived project brain.

Recommended chat roles:

- Control Room: product direction, priorities, architecture decisions and review of Codex results.
- Research: Enter the Gungeon internals, modding APIs, Steam networking constraints and external references.
- Implementation: preparing precise Codex tasks and reviewing diffs/PRs.
- Testing: logs, reproduction steps, bug triage and playtest findings.

Important architecture decisions should be written back into the repository as ADRs rather than living only in chat history.

## Codex

Codex is the implementation agent.

Before assigning implementation work:

1. Check out the intended spike/feature branch.
2. Ensure Codex reads `AGENTS.md`, `PROJECT.md` and the relevant spike/ADR document.
3. Give Codex one bounded task with acceptance criteria.
4. Ask it to build/test where possible and report blockers rather than broadening scope.
5. Review the result in the Control Room before starting the next architectural step.

## Branch model

- `main`: upstream/reference baseline for now.
- `project/planning`: planning source of truth.
- `spike/*`: disposable technical experiments.
- Future production branch/repository: created after Phase 0 architecture is validated.

Spikes may be ugly internally if necessary to answer a technical question, but findings must be documented before any code is promoted into the production architecture.

## Decision records

Create `docs/ADR-XXX-name.md` for decisions that materially constrain future implementation.

Each ADR should contain:

- Status
- Context
- Decision
- Alternatives considered
- Consequences
- Evidence/spike that supports it

## Source-of-truth order

When instructions conflict, use this order:

1. Current accepted ADRs
2. `PROJECT.md`
3. Current spike brief
4. `AGENTS.md`
5. Existing implementation

Existing GungeonTogether behavior is evidence, not a requirement.
