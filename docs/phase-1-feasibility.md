# Phase 1 — Feasibility and technical foundation

<badge>COMPLETE — source research</badge>

**Target:** physical New Nintendo 2DS XL (Luma3DS) ↔ Azahar on Android.  
**Result:** technically feasible. The required bridge architecture is documented by the existing 3DS Link implementation. A fresh implementation is possible, but it would duplicate substantial existing work.

## 1. Verified technical findings

### Physical console side
- Luma3DS supports game plugins in 3GX format through its Plugin Loader.
- 3DS Link's plugin takes the place of the game's `nwm::UDS` wireless service and handles the local-wireless calls.
- It converts wireless beacons, data frames, and node information into ENet packets for an Azahar multiplayer room.

### Azahar / Android side
- Azahar emulates the 3DS local-wireless UDS service and uses room networking for multiplayer.
- Azahar's Android multiplayer release notes explicitly state that Android room support does **not by itself** implement communication with real consoles.
- Therefore, an Android room alone is not the bridge. A console-side plugin compatible with Azahar's room protocol is required.

### Existing implementation evidence
- **3DS Link** documents the complete physical-console ↔ Azahar-room design: Luma 3GX plugin, configuration app, and room transport.
- **CTRelay** documents a different relay architecture: Luma-side UDS redirection, UDP relay, server-side room shim, and Azahar room.
- These projects establish that the architecture is technically possible; they are reference implementations, not proof that XAVRX has been built or tested.

## 2. Selected XAVRX architecture

```
New 2DS XL game
      |
      v
Luma3DS 3GX plugin
(intercepts nwm::UDS)
      |
      v
ENet / Azahar room protocol
      |
      v
Azahar Android
(emulated NWM_UDS)
      |
      v
Android game session
```

**Decision:** use an Azahar-compatible room transport. The first prototype should prioritize same-room connectivity and one supported local-multiplayer title. Do not begin with a custom internet relay; it adds complexity before the basic data path is proven.

## 3. Compatibility and limitations

- Not every game uses local wireless in the same way. Compatibility must be checked title by title.
- Local wireless, Download Play, and Nintendo online services are different paths; success in one does not prove the others.
- A room ping or a brief join notification is not a successful multiplayer test.
- Android Azahar's built-in multiplayer feature alone is insufficient for a physical console.
- The user's New 2DS XL and Android phone still need an actual end-to-end test before any game can be declared working.

## 4. Licensing and reuse

The 3DS Link project states that it is licensed under GPL-2.0-or-later and includes code derived from Azahar. If XAVRX reuses or modifies that code, preserve the applicable copyright notices, license text, and source-offer obligations. Do not copy code blindly without checking each component's license.

## 5. Phase 1 exit checklist

- [x] Identified the physical-console interface: Luma3DS 3GX plugin and `nwm::UDS` interception.
- [x] Identified the Azahar interface: emulated UDS service and ENet-based room networking.
- [x] Documented the compatibility gap: Android room support alone does not connect real consoles.
- [x] Selected a minimal architecture: console plugin → Azahar room transport → Android Azahar.
- [x] Defined the first test: both devices join the same session and exchange gameplay data continuously.
- [x] Documented game-specific limitations and licensing requirements.

## 6. Phase 1 conclusion

**PHASE 1 IS COMPLETE at the research/design level.** The architecture is technically credible and has existing public implementations to study. No XAVRX executable, plugin, Android app, or successful gameplay connection has been created yet.

**Phase 2 can begin:** build the smallest prototype by studying/reusing the documented Azahar-compatible UDS-to-room approach, while respecting licenses. The first milestone is a stable console plugin connection to an Azahar room—not a finished multiplayer product.

## 7. Sources

1. 3DS Link project and architecture: https://github.com/gionko/3DS-Link
2. CTRelay project and relay architecture: https://github.com/Zeusfail/CTRelay
3. Luma3DS project and 3GX plugin support: https://github.com/LumaTeam/Luma3DS
4. Azahar Android multiplayer release notes (Android room support limitation): https://sourceforge.net/projects/azahar.mirror/files/2126.0/
