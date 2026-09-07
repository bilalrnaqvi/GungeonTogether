# Codex Task 001: Transport reconnaissance

## Goal

Do not implement gameplay. Determine what Steam networking capabilities are actually available from the Enter the Gungeon mod runtime and prepare the smallest clean transport experiment.

## Read first

- `AGENTS.md`
- `PROJECT.md`
- `docs/ADR-001-foundation-strategy.md`
- `docs/SPIKE-001-TRANSPORT.md`

## Task

Inspect the current repository and its referenced assemblies. Determine:

1. Which Steamworks assembly/types the game exposes at runtime.
2. Whether SteamNetworkingMessages APIs appear available.
3. Whether SteamNetworkingSockets APIs appear available.
4. What the inherited `SteamP2PManager` depends on and which parts are reusable as compatibility/reference code.
5. What build/runtime constraints exist because the mod targets the game's older .NET/Unity environment.

Then propose the smallest implementation plan for an `IPeerTransport` spike.

Do not refactor the existing multiplayer systems yet. Do not implement player/world/combat sync.

## Output

Create `docs/SPIKE-001-RECON.md` with:

- available Steamworks types/APIs discovered from the project/game references
- uncertainties that require runtime testing
- recommended first transport implementation to test
- files that would be added/modified
- exact manual test procedure
- any dependency/deployment implications

If a tiny compile-only probe is required to answer an API availability question, isolate it clearly and explain why it exists.

## Completion report

At the end, summarize:

- findings
- files changed
- build result
- what still requires running the game
- recommended next Codex task
