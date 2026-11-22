# Phase 2 — Connection prototype

Status: **in progress — interface confirmed, bridge not yet implemented**

## Source-backed findings

Research date: 2026-11-21.

### Azahar side

- The local wireless emulation is implemented in `src/core/hle/service/nwm/nwm_uds.cpp`.
- Its `SendPacket(Network::WifiPacket&)` path forwards packets through `Network::GetRoomMember()` only while the room member is in `Joined` or `Moderator` state.
- The room client implementation in `src/network/room_member.cpp` uses ENet (`ENetHost` / `ENetPeer`) and dispatches received `IdWifiPacket` messages to the Wi-Fi packet callback.
- Therefore the Azahar bridge point is not the Android room form or a generic UDP socket: it is the boundary between NWM UDS Wi-Fi packets and the RoomMember ENet transport.

Primary sources:
- https://github.com/azahar-emu/azahar/blob/master/src/core/hle/service/nwm/nwm_uds.cpp
- https://github.com/azahar-emu/azahar/blob/master/src/network/room_member.cpp
- https://github.com/azahar-emu/azahar/blob/master/src/network/room_member.h

### Physical console side

- Luma3DS documents support for game plugins (3GX), patching/replacing system modules, and kernel/IPC hooks.
- This confirms extension mechanisms exist, but does **not** prove that a normal 3GX plugin can intercept all NWM UDS traffic or provide a stable packet bridge.
- The physical-side prototype must establish a supported hook point for NWM/UDS packet send/receive, or use a purpose-built system-module component. A normal game plugin alone is not assumed sufficient.

Primary source:
- https://github.com/LumaTeam/Luma3DS

## Prototype architecture

```text
Physical 2DS XL
  Game -> NWM/UDS -> physical-side capture/adapter
                         |
                    bridge framing
                         |
                    local/WAN relay
                         |
                  Azahar-side adapter
                         |
               NWM WifiPacket boundary
                         |
                  Azahar RoomMember
```

The relay transports opaque, framed payloads only. It cannot translate incompatible UDS state machines, node IDs, MAC identities, beacon discovery, or connection handshakes by itself. Those semantics must be mapped by the two endpoint adapters.

## Phase 2 implementation sequence

1. Build a packet-format inventory from Azahar's `WifiPacket`, `IdWifiPacket`, and UDS handlers.
2. Identify a physical Luma-side interception point for outgoing and incoming UDS traffic.
3. Define a versioned bridge frame with peer identity, direction, sequence number, payload length, and integrity checks.
4. Implement a loopback relay/probe and test framing independently of gameplay.
5. Add endpoint adapters only after the two packet boundaries are confirmed.

## Acceptance tests

- A packet emitted by one endpoint is received byte-for-byte by the other through the prototype transport.
- Peer identity and direction remain unambiguous.
- Disconnect/reconnect does not leave stale peer state.
- A real game session reaches discovery and handshake; successful ping or lobby join alone does not count.
- Sustained gameplay data exchange is required before declaring compatibility.

## Current blocker

The Azahar-side network boundary is confirmed in source. The physical-side NWM/UDS interception mechanism is not yet confirmed. Consequently, this document records a concrete prototype design, not a completed or working bridge. Do not start Phase 3 until Phase 2 has an executable prototype and the required packet-path tests pass.
