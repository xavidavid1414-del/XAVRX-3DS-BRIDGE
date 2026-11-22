# XAVRX-3DS-BRIDGE

A research and development project exploring multiplayer connectivity between a **real Nintendo 3DS-family console** (New Nintendo 2DS XL with Luma3DS) and **Azahar on Android**.

## Goal

Investigate whether a standalone bridge can make compatible local-wireless multiplayer sessions work between:
- a physical New Nintendo 2DS XL; and
- an Android phone running Azahar.

This project is specifically **not** an emulator-to-emulator networking tool.

## Three-phase plan

1. **Phase 1 — Feasibility and foundation**
   - Document the 3DS local wireless protocol and Azahar networking interfaces.
   - Identify what can be reused and what must be implemented.
   - Define a minimal test case and a safe, testable architecture.
   - Exit gate: demonstrate a technically credible path before claiming compatibility.
2. **Phase 2 — Connection prototype**
   - Implement the smallest end-to-end connection prototype supported by Phase 1 findings.
   - Keep console, Android, and relay components separated where needed.
3. **Phase 3 — Validation**
   - Test with a specific supported game and hardware setup.
   - Record reproducible results, limitations, and fixes.
   - Do not claim a game works until a real test succeeds.

## Important constraints

- A network relay alone does not automatically translate Nintendo 3DS local wireless traffic into an emulator's networking protocol.
- Game compatibility may vary; local multiplayer, Download Play, and online services are distinct cases.
- No compatibility claims are made until verified on the physical console and Android device.
- The project must not bypass Nintendo account protections or use copyrighted game files.

## Current status

**Phase 1: started — documentation and feasibility research.**

Hardware target:
- Console: New Nintendo 2DS XL, Luma3DS.
- Android: Azahar.
- Desired link: physical console ↔ Android emulator.

## Repository structure (planned)

```
docs/
  phase-1-feasibility.md
console/
android/
relay/
tests/
```

Implementation directories will be added only after Phase 1 establishes the required interfaces and protocol boundaries.
