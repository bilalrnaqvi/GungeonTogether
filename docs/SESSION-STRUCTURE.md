# Session Structure

## ChatGPT Project

Keep one ChatGPT Project for the entire online co-op effort.

Recommended chats:

1. `00 Control Room` — project direction, architecture decisions, milestone status, review of Codex work.
2. `01 Research` — Enter the Gungeon internals, modding constraints, Steam networking and external technical references.
3. `02 Implementation Review` — prepare Codex tasks, inspect diffs and decide what gets accepted.
4. `03 Testing & Bugs` — paste logs, reproduction steps, screenshots and playtest findings.

The Control Room is the canonical conversational thread. Important conclusions must still be written into repository docs/ADRs.

## Codex

Use one Codex workspace pointed at the repository.

For each spike or feature:

- check out the corresponding branch
- let Codex read root `AGENTS.md`
- point it to the current task brief
- give it one bounded objective
- return results to the ChatGPT Control Room for architecture review

Avoid one giant Codex conversation that tries to design and implement the entire mod. Use short task-focused sessions tied to repository documents.

## Current next session

Branch: `spike/001-transport`

Task brief: `docs/CODEX-TASK-001.md`

Expected first outcome: reconnaissance and a written transport recommendation, not gameplay code.
