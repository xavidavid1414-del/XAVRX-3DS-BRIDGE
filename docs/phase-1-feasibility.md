# Phase 1 — Feasibility and technical foundation

**Status:** in progress  
**Target:** a physical New Nintendo 2DS XL ↔ Azahar on Android  
**Rule:** do not start Phase 2 until the feasibility questions below have evidence-based answers.

## 1. Problem statement

The two endpoints do not automatically speak the same multiplayer transport merely because both support Nintendo 3DS games. The project must determine how a physical console's local-wireless traffic is represented and how Azahar exposes or receives equivalent traffic.

A generic TCP/UDP relay is not sufficient unless both endpoints already use compatible packet formats and session semantics.

## 2. Protocol and interface questions

### Physical console
- Which Luma3DS extension point can observe or provide the required local-wireless operations?
- Is the target traffic UDS (local wireless), and which operations are required for discovery, session creation, data transfer, and teardown?
- Can a homebrew/plugin component access those operations reliably without replacing unrelated system behavior?

### Android / Azahar
- Which networking backend and room protocol does the Android build expose?
- Can an external client or plugin connect to that backend, or would Azahar itself need a code change?
- Does Android support the same local-wireless session semantics as the physical console, rather than only emulator-to-emulator rooms?

### Transport and relay
- Which messages must be forwarded unchanged, and which require translation?
- How are peers discovered, sessions identified, and disconnects handled?
- Can the first prototype work on one Wi-Fi network before attempting internet relay?

## 3. Proposed investigation sequence

1. Read the current Luma3DS plugin/homebrew interfaces relevant to wireless networking.
2. Read Azahar's Android multiplayer implementation and identify its actual transport boundary.
3. Trace the existing 3DS Link and CTRelay designs as references, not as assumed dependencies.
4. Build a protocol compatibility table: operation, physical-console API, Azahar API, translation required, evidence.
5. Select one simple local-multiplayer title only after the transport path is understood.

## 4. Phase 1 exit criteria

Phase 1 is complete only when all are true:

- [ ] The physical-console networking interface is identified from source or reliable technical documentation.
- [ ] The Azahar Android networking interface is identified from source or reliable technical documentation.
- [ ] The compatibility gap between the two is documented.
- [ ] A minimal end-to-end architecture is selected, including whether Azahar must be modified.
- [ ] A realistic first test and measurable success condition are defined.
- [ ] Known limitations are written down.

If the interfaces cannot be bridged safely or technically, stop and report that result rather than pretending the prototype is viable.

## 5. Initial architecture candidates

These are hypotheses, not decisions:

- **Direct LAN bridge:** only viable if the two endpoints expose compatible transport and session semantics.
- **Relay with protocol adapter:** viable if the console-side and Azahar-side interfaces can both be accessed and translated.
- **Azahar-side integration:** required if Azahar Android does not expose a usable external networking interface.

## 6. First test definition (provisional)

- Both devices on the same Wi-Fi network.
- One physical New Nintendo 2DS XL and one Android device running Azahar.
- One title with documented local multiplayer support.
- Success means both devices discover/join the same session and exchange gameplay data for a sustained match; a room ping or temporary join notification does not count.

## 7. Evidence log

Add dated source links and concise findings here as the investigation proceeds. Distinguish direct source-code evidence, official documentation, and community reports.
