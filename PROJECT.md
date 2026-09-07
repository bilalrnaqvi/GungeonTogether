# Enter the Gungeon Online Co-op Project

## Working goal

Build a native-feeling online co-op mod for Enter the Gungeon where each player runs their own game client, joins through Steam, controls their own Gungeoneer, and participates in the same synchronized run.

## Current status

Planning and architecture validation only. No gameplay implementation should be merged until the foundation decision and technical spikes are complete.

## Product principles

1. Online co-op should feel like a real multiplayer mode, not remote-play streaming.
2. Each player owns their own local input and camera.
3. Shared game state should have explicit authority and ownership rules.
4. Networking should be replaceable behind a transport abstraction.
5. Synchronization should be event-driven where possible and snapshot-driven where necessary.
6. Debugging, logging, protocol versioning, and desync detection are first-class systems.
7. We should prove risky systems with small technical spikes before building the full run loop.

## Foundation decision

GungeonTogether is retained as a reference implementation and potential source of reusable MIT-licensed components. It should not automatically define the architecture of the new project.

The leading strategy is a clean architecture that may selectively port proven pieces from GungeonTogether after review.

See `docs/ADR-001-foundation-strategy.md`.

## Proposed architecture

- `Core/` mod bootstrap, lifecycle, configuration
- `Transport/` abstract peer transport and Steam implementation
- `Protocol/` packet definitions, serialization, protocol compatibility
- `Session/` lobby membership, connection state, ownership, host authority
- `Players/` remote player representation and player replication
- `World/` floor, room, door, chest, pickup and run-state replication
- `Combat/` enemy, projectile, damage and combat-event replication
- `Integration/` Enter the Gungeon hooks and adapters
- `Diagnostics/` logging, packet statistics, desync checks and debug UI
- `UI/` host/join flow, player list and multiplayer settings

## Authority model

Initial target: host-authoritative shared world.

- Local client owns local input.
- Host owns canonical shared world state.
- Remote player presentation is replicated from network state.
- Clients request shared-world actions, host validates and publishes the result.
- Reliable delivery is reserved for state transitions and important events.
- High-frequency movement uses unreliable snapshots plus interpolation.

## Phase 0: technical proof

Before building full gameplay, prove these independently:

1. Two game clients can connect and exchange versioned packets reliably.
2. A remote Gungeoneer can be represented correctly with character identity, movement, facing and animation.
3. Both clients can enter the same run and floor without corrupting local state.
4. One simple combat room can remain synchronized through enemy spawn, damage and room clear.
5. We can choose a projectile replication model that remains practical under Gungeon's bullet density.

## First playable definition

A host and one client can:

- join one Steam session
- choose separate characters
- enter the same run
- see each other move, aim, roll and shoot
- fight through a basic room together
- transition to another room without desync

No matchmaking, voice chat, custom game modes, unlimited players, or cross-store networking are required for the first playable.

## Repository policy during planning

- `main` remains unchanged.
- `project/planning` contains planning documents only.
- Experimental implementation work should happen in disposable spike branches until architecture decisions are settled.
- The existing `dev/remote-player-foundation` branch is frozen and should not receive further work for now.
