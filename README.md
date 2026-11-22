# XAVRX-3DS-BRIDGE

A research and development project for multiplayer connectivity between a **physical New Nintendo 2DS XL running Luma3DS** and **Azahar on Android**.

## Goal
Connect a real 3DS-family console with an Android Azahar instance in the same local-wireless game session. This is not an emulator-to-emulator project.

## Three phases
1. **Phase 1 — Feasibility and foundation: COMPLETE.** Research established a technically credible architecture based on a Luma3DS 3GX plugin intercepting `nwm::UDS` and forwarding compatible packets through an Azahar room.
2. **Phase 2 — Connection prototype: NEXT.** Build the smallest console-side plugin / room-connection prototype, respecting upstream licenses.
3. **Phase 3 — Validation.** Test a real game on the physical New 2DS XL and Android Azahar; publish only verified compatibility results.

## Important facts
- Azahar Android multiplayer rooms do not by themselves connect real consoles.
- A console-side Luma3DS plugin is required to bridge local-wireless UDS traffic to Azahar's room transport.
- Game compatibility varies. Local wireless, Download Play, and online services are separate cases.
- No XAVRX app, plugin, or working gameplay connection is claimed yet.

## Hardware target
- Console: New Nintendo 2DS XL with Luma3DS.
- Android: phone running Azahar.

## Project status
**Phase 1 complete — Phase 2 is ready to begin.**

See [Phase 1 technical findings](docs/phase-1-feasibility.md).
