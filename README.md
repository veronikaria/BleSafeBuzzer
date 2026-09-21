# SafeGuard Buzzer v1 — CR2450 One-Side Assembly

SafeGuard Buzzer is a BLE-controlled backpack or laptop-case alarm module paired with SafeGuard Ping. It uses a passive magnetic sounder so firmware can generate alarm patterns and escalation. KiCad is the source of truth for the electrical and PCB design.

## Current hardware

- BLE module: Renesas/Dialog DA14531MOD-00F01002, LCSC C5360767
- Battery: replaceable CR2450 primary coin cell
- Battery holder: MYOUNG MY-2450-03, LCSC C2988622
- Buzzer: FUET FUET-1365-3V passive magnetic SMD sounder, LCSC C417422
- Power switch: PCM12SMTR, LCSC C221841
- Buzzer driver: AO3400A N-channel MOSFET, LCSC C20917
- Clamp diode: BAT54WS, LCSC C22629
- Status indicator: red 0603 LED with 2.2 kΩ resistor
- Debug: compact VCC, GND, SWDIO, and SWCLK pads
- Current measurement: bridgeable JP1 link
- No charger, USB, user silence/test button, or external reset supervisor

Power path:

```text
CR2450 -> S1 ON/OFF switch -> JP1 current-measurement bridge -> VCC
```

Buzzer drive path:

```text
DA14531MOD PWM/GPIO -> 100 ohm gate resistor -> AO3400A -> passive buzzer
                                      |
                                  100 kohm pulldown
```

## Mechanical configuration

- Board size: 65.0 × 40.0 mm
- BT1: Top at `(24.0, 24.0)`
- BZ1: Top at `(48.0, 31.0)`, near the edge for a future hidden acoustic opening
- S1: Top at `(6.0, 7.0)`, left-edge accessible
- U1: Top at `(54.5, 10.5)`, with the antenna at the board edge
- All 13 assembled BOM/CPL components are on Top
- Bottom contains copper and traces only; it has no assembled components or footprints
- Battery metal does not overlap the DA14531MOD antenna keepout

The PCB remains a two-copper-layer design, but JLC assembly is **Top Side only**.

## Active KiCad files

- Schematic: `SafeGuardBuzzer_v1.kicad_sch`
- PCB: `SafeGuardBuzzer_v1.kicad_pcb`
- Project-local footprints: `SafeGuardBuzzer_Local.pretty/`
- Project-local symbols: `SafeGuardBuzzer_Local.kicad_sym`

## Validation status

- ERC: 0 errors and 10 footprint-library configuration warnings
- DRC: 0 real electrical or fabrication errors
- Unconnected items: 0
- Shorts: 0
- Clearance errors: 0
- Antenna-keepout violations: 0
- Remaining DRC warnings: missing configured stock footprint libraries, one U1 library-copy mismatch, and two clipped-silkscreen warnings
- Schematic parity warnings are metadata/value differences and do not change connectivity or the verified manufacturing BOM/CPL

## JLC manufacturing outputs

Output folder:

```text
Manufacturing_Final_Buzzer_v1_CR2450_OneSide_JLC/
```

Upload these files:

- Gerber/drill ZIP: `SafeGuardBuzzer_v1_CR2450_OneSide_JLC_Gerber_Drill_Upload_CORRECTED.zip`
- BOM: `SafeGuardBuzzer_v1_CR2450_OneSide_JLC_BOM.csv`
- CPL: `SafeGuardBuzzer_v1_CR2450_OneSide_JLC_CPL.csv`

Do not upload `SafeGuardBuzzer_v1_CR2450_OneSide_JLC_Gerber_Drill_Upload.zip`. That older archive contains `SafeGuardBuzzer_v1-drl_map.gbr`, whose legend extends outside the PCB and can cause JLCPCB to report an incorrect 75 × 70 mm size.

The corrected ZIP contains only the fabrication Gerbers, the actual Excellon drill file, and the Gerber job file. Its Edge.Cuts profile is exactly 65 × 40 mm.

J1, J2, and JP1 are copper-only debug/test/current-measurement pads and are intentionally excluded from both BOM and CPL.

## JLC order settings and preview checklist

1. Select a two-layer PCB.
2. Select **Top Side only** assembly.
3. Confirm detected board size is 65 × 40 mm.
4. Confirm all 13 CPL placements are Top; Bottom should contain no placements.
5. Do not accept automatic component alignment if the uploaded CPL already overlays the Gerbers correctly.
6. Confirm BT1 is MY-2450-03 / C2988622, BZ1 is FUET-1365-3V / C417422, S1 is PCM12SMTR / C221841, and U1 is DA14531MOD / C5360767.
7. Confirm buzzer, diode, LED, MOSFET, switch, and module orientation in the placement preview.
8. Confirm the DA14531MOD antenna keepout remains free of copper and battery metal.

## Prototype note

CR2450 cells have less pulse-current capability than CR123A cells. The selected buzzer and bulk capacitance are intended for a moderate alarm, but sound pressure and battery droop should be measured on assembled prototypes during a full one-to-two-minute alarm cycle.

