# Spike 001: Transport

## Purpose

Determine the networking transport foundation before building any gameplay synchronization.

## Context

The inherited GungeonTogether implementation uses the older Steam P2P API through reflection. This spike should evaluate what is actually practical inside Enter the Gungeon's runtime rather than assuming the inherited transport should remain.

## Required design

Create a transport abstraction that gameplay code can depend on without knowing which Steam API is underneath it.

Conceptual surface:

```csharp
public interface IPeerTransport
{
    ulong LocalPeerId { get; }
    bool IsReady { get; }

    event Action<ulong> PeerConnected;
    event Action<ulong, byte[]> MessageReceived;
    event Action<ulong, string> PeerDisconnected;

    void Initialise();
    void Update();
    void Send(ulong peerId, byte[] payload, DeliveryMode delivery);
    void Disconnect(ulong peerId);
    void Shutdown();
}
```

The exact interface may change during the spike. Keep it small.

## Evaluate

1. Current inherited Steam P2P implementation.
2. SteamNetworkingMessages if accessible in the game's bundled Steamworks environment.
3. SteamNetworkingSockets if accessible and reasonable for peer-hosted co-op.

Do not add a new native dependency simply to make a modern API available without documenting the deployment implications first.

## Minimal protocol

Implement only enough protocol to test the transport:

- Hello / protocol version
- HelloAccepted
- HelloRejected
- Ping
- Pong
- Disconnect reason

Use an explicit packet header with protocol version and message type.

## Diagnostics

Log at minimum:

- transport initialization result
- local Steam ID
- connection/session request
- accepted/rejected connection
- send failures
- decode failures
- disconnect/failure reason
- measured ping round trip

Do not silently swallow exceptions.

## Test matrix

### Local development

- Game process A hosts.
- Game process B connects.
- Reliable ping/echo.
- Unreliable ping/echo.
- Intentional protocol mismatch.
- Client closes unexpectedly.
- Host closes unexpectedly.

### Network

Repeat host/client connection on two separate Steam accounts/machines when available.

## Explicitly out of scope

- player synchronization
- dungeon synchronization
- enemies
- projectiles
- inventory
- multiplayer UI beyond minimal debug controls
- matchmaking

## Deliverable

At the end of the spike, create `docs/SPIKE-001-RESULTS.md` containing:

- APIs tested
- what worked and failed
- compatibility/runtime constraints
- recommended transport choice
- remaining risks
- packet/ping observations
- recommendation: keep, rewrite or adapt inherited Steam transport

Do not proceed to Spike 002 until this result is reviewed.
