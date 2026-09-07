# AGENTS.md

## Project mission

Build a native-feeling online co-op mod for Enter the Gungeon. Each player runs their own game client, controls their own Gungeoneer and camera, and participates in one synchronized run.

## Current phase

We are in Phase 0: architecture validation and technical spikes.

Do not expand scope into full gameplay systems until the current spike has been proven and documented.

## Foundation strategy

The existing GungeonTogether codebase is reference material and a source of selectively reusable MIT-licensed components. Do not assume its architecture is correct or preserve it for compatibility.

Prefer clean interfaces and small adapters. Port existing code only when it has a clear benefit over rewriting it.

## Core architecture rules

1. Local input is owned by the local client.
2. Shared world state is initially host-authoritative.
3. Remote players must never overwrite or impersonate the local PlayerController.
4. Networking transport must sit behind an interface so Steam implementation details do not leak into gameplay synchronization code.
5. Protocol messages must be versioned and explicitly serialized.
6. Reliable delivery is for important state transitions and commands that must arrive.
7. High-frequency transient state such as movement should use unreliable snapshots plus interpolation where practical.
8. Avoid networking every projectile transform every frame. Projectile replication strategy will be validated separately.
9. Diagnostics are mandatory. Do not silently swallow networking exceptions.
10. Prefer deterministic IDs and explicit ownership over scene searches and implicit object identity.

## Proposed module boundaries

- Core: bootstrap, lifecycle, config
- Transport: peer connectivity and Steam implementation
- Protocol: messages, serialization, compatibility/versioning
- Session: lobby membership, connection lifecycle, roles and ownership
- Players: remote player representation and player replication
- World: floor, room, door, chest, pickup and run-state replication
- Combat: enemies, projectiles, damage and combat events
- Integration: Enter the Gungeon hooks/adapters
- Diagnostics: logs, packet metrics, desync checks, debug UI
- UI: host/join flow, multiplayer settings and player list

## Development workflow

- Read PROJECT.md before making architectural changes.
- Read the relevant ADR before changing a decision it covers.
- Each risky subsystem starts as a focused spike branch.
- Keep spikes small and measurable.
- After a spike, document findings before building on it.
- Do not merge experimental hacks into the eventual production architecture simply because they worked once.

## Current success target

The first playable eventually needs two Steam players to join one session, choose separate characters, enter the same run, see each other move/aim/roll/shoot, clear a basic room together and transition rooms without desync.

## Coding expectations

- C# targeting the Enter the Gungeon/BepInEx environment used by the repository.
- Keep dependencies minimal.
- Use clear names instead of clever abstractions.
- Avoid broad reflection helpers when a narrow adapter can isolate a specific game API dependency.
- Add logs around connection lifecycle, packet decode failures, authority decisions and synchronization transitions.
- Never catch Exception without either logging useful diagnostic context or deliberately rethrowing.

## Before completing a Codex task

Report:

1. Files changed.
2. What behavior was implemented.
3. What was deliberately left out.
4. How to build/test it.
5. Any architecture questions or discoveries that should become an ADR.
