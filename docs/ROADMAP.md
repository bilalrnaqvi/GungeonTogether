# Roadmap

## Phase 0: Architecture validation

Goal: prove the risky technical assumptions before committing to the production architecture.

### Spike 001: Transport

Questions:
- Which Steam networking API can be used reliably from Enter the Gungeon's runtime?
- Can two game clients exchange versioned reliable and unreliable messages?
- Can connection/disconnection failures be surfaced cleanly?
- Can transport details remain behind a small interface?

Exit criteria:
- Host and client connect from separate game processes/machines.
- Ping/echo messages work in both reliable and unreliable modes.
- Protocol mismatch is rejected cleanly.
- Disconnect is detected and logged.
- No gameplay synchronization is included.

### Spike 002: Remote Gungeoneer

Questions:
- How should a remote player be represented without corrupting local PlayerController state?
- Can character identity, sprite/material, animation, facing, aim and dodge state be reproduced correctly?
- What snapshot rate/interpolation strategy feels acceptable?

Exit criteria:
- Two clients can stand in the Breach and see accurate remote characters.
- No combat or run synchronization required.

### Spike 003: Run/floor synchronization

Questions:
- How do we make both clients enter equivalent dungeon state?
- Can the host provide a seed/state transition instead of teleport-driven correction?
- Which transitions require barriers/readiness acknowledgements?

Exit criteria:
- Both clients enter the same test run/floor and arrive in a consistent starting state.

### Spike 004: One combat room

Questions:
- How should enemy identity and authority work?
- Which events are host-authored versus client-requested?
- How are damage and room clear replicated?

Exit criteria:
- One simple room can be cleared by two players without divergent enemy/room state.

### Spike 005: Projectile model

Questions:
- Which projectiles can be reconstructed from fire events and deterministic parameters?
- Which need snapshots or explicit spawn events?
- What happens under bullet-heavy boss patterns?

Exit criteria:
- A documented projectile replication strategy with measured packet/runtime behavior.

## Phase 1: Vertical slice

Combine proven spike results into one production-shaped implementation.

Target:
- Steam host/join
- two characters
- same run/floor
- movement, aim, roll and shooting
- basic enemy combat
- room transition
- reconnect/quit cleanup sufficient for testing

## Phase 2: Core run completeness

Add:
- pickups and inventory changes
- chests
- shops
- keys/currency
- active/passive items
- enemy variety
- bosses
- floor transitions
- deaths/revive design

## Phase 3: Productization

Add:
- polished multiplayer UI
- configuration/difficulty scaling
- diagnostics suitable for player bug reports
- packaging/build pipeline
- compatibility testing
- documentation

## Deferred until core multiplayer works

- matchmaking
- voice chat
- cross-store networking
- unlimited players
- custom multiplayer game modes
- large balance redesigns
