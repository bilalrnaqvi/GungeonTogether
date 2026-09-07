# ADR-001: Foundation Strategy

## Status

Proposed

## Context

We want to build robust online co-op for Enter the Gungeon. An existing MIT-licensed project, GungeonTogether, already demonstrates useful concepts including Steam lobbies, P2P connections, packet serialization, host/client roles and early player/world synchronization.

However, the current implementation also carries architectural and implementation risk:

- networking and game synchronization are tightly coupled
- Steam transport uses the older ISteamNetworking P2P API through extensive runtime reflection
- network receive paths suppress some exceptions, which makes failures difficult to diagnose
- player synchronization has unclear ownership boundaries in unfinished code
- remote players are currently presentation objects rather than a robust replicated player model
- world synchronization mixes player position with world transition detection
- world-state application contains duplicate apply paths and timing-based teleport logic
- synchronization systems are instantiated as global persistent MonoBehaviours, increasing hidden coupling
- existing project issues report player visibility, transition, interpolation, character-sync and cleanup problems

Starting completely from zero would also waste useful reverse-engineering work and proven integration knowledge.

## Options considered

### A. Continue the fork directly

Pros:
- fastest route to visible progress
- Steam lobby and packet plumbing already exists
- existing Enter the Gungeon reflection/integration work can be reused immediately

Cons:
- difficult to separate architectural decisions from experimental code
- accumulated synchronization assumptions may become expensive to unwind
- legacy transport and reflection-heavy implementation become foundational dependencies
- debugging behavior is already difficult in some network paths

### B. Start completely from scratch

Pros:
- clean ownership and authority model from day one
- transport abstraction and testing can be designed deliberately
- no inherited coupling

Cons:
- repeats solved reverse-engineering work
- higher initial effort
- easy to rediscover old integration problems

### C. Clean architecture with selective reuse

Create a new architecture and treat GungeonTogether as a reference implementation and MIT-licensed code donor. Port components only after they have been reviewed against the new interfaces and ownership model.

Pros:
- keeps useful discoveries without inheriting the whole architecture
- lets transport, protocol and game synchronization evolve independently
- provides a clear path to replacing old Steam networking later
- easier to test and reason about authority

Cons:
- slower initial visible progress than simply patching the fork
- requires discipline around selective porting

## Decision

Use **Option C: clean architecture with selective reuse** unless technical spikes reveal a strong reason to change course.

The existing fork remains valuable as:

1. a reference for Enter the Gungeon integration points
2. a reference for Steam lobby behavior
3. a collection of experiments that show what has and has not worked
4. a source of MIT-licensed components that can be ported deliberately

It should not be treated as the canonical architecture.

## Reuse candidates

Potentially reusable after review:

- Steam lobby discovery/invite behavior
- reflection helpers for locating Steamworks types in Enter the Gungeon
- portions of packet serialization
- UI concepts and game integration discoveries
- existing knowledge about foyer/run transitions

Likely rewrite areas:

- session state machine
- player ownership and replication
- world synchronization
- enemy synchronization
- transport interface
- diagnostics/error handling
- synchronization scheduling

## Validation required before acceptance

1. Determine which Steam networking API is practical inside Enter the Gungeon's runtime and mod environment.
2. Verify whether a modern transport can be used directly or whether a legacy-compatible adapter is necessary.
3. Prototype a real remote Gungeoneer representation without mutating the receiving client's local player state.
4. Prototype deterministic or host-directed floor synchronization.
5. Measure projectile/event traffic in representative combat before selecting the projectile replication strategy.

## Consequences

No large gameplay feature should be built on the existing synchronization code until these spikes are complete. Any copied GungeonTogether code must retain the required MIT license notice and be adapted behind the new project's interfaces where appropriate.
