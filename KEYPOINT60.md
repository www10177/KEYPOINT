# KEYPOINT60 variant

This branch is the first-pass conversion of KEYPOINT from the stock 48-key split typing
layout to a 60-key split typing layout with a dedicated number row.

## Layout goal

The new physical typing layout adds six switches per half:

```text
Left                                  Right
Esc  1  2  3  4  5        |        6  7  8  9  0  Backspace
Tab  Q  W  E  R  T        |        Y  U  I  O  P  Backspace
Caps A  S  D  F  G        |        H  J  K  L  ;  Enter
Shft Z  X  C  V  B        |        N  M  ,  .  /  Shft
        existing thumb / pointing controls retained
```

This gives **60 primary mechanical typing keys** (48 original + 12 number-row keys).
The existing center auxiliary mouse/scroll buttons and thumb controls remain separate,
so the ZMK physical layout contains 68 total logical positions.

This is a split 60-key layout, not an ANSI 60% plate geometry.

## Matrix allocation

No new MCU GPIOs are required in the firmware design.

The stock KEYPOINT matrix is already declared as six rows by eight columns per half.
The new number-row positions use previously unused row-5 intersections:

- Left number row: `RC(5,0)` through `RC(5,5)`
- Right number row: `RC(5,8)` through `RC(5,13)`
- Existing auxiliary switches retain `RC(5,6)`, `RC(5,7)`, `RC(5,14)`, and `RC(5,15)`

## Firmware changes in this branch

- Extended the ZMK matrix transform from 56 to 68 logical positions.
- Added a 12-key number row to every keymap layer.
- Extended the physical-layout metadata so ZMK Studio/keymap tooling sees the extra row.
- Shifted the hard-coded A320/TrackPoint position listeners by 12 positions so the
  existing scroll/slow/arrow behaviors continue to refer to the same original keys.

## Hardware status

**This branch does not make the current production PCB grow twelve switches.**

A new PCB/revision is still required to physically route the twelve new switch
intersections to row 5 and columns 0-5 on each half. The public upstream repository
does not currently include the original KiCad schematic/PCB/Gerber/BOM, so the PCB
will need to be recreated from the published firmware pin map and mechanical files.

Likewise, the current STEP top cases are still the upstream 48-key mechanical design
and need a KEYPOINT60 case revision after the PCB outline and switch placement are
locked.

## Next hardware pass

1. Recreate the left/right schematic from the published DTS pin assignments.
2. Add the six number-row switches on each half to the existing 6x8 matrices.
3. Route a first KiCad PCB while retaining TrackPoint, A320 trackpad, encoders,
   memory LCD, battery, USB, and charging circuitry.
4. Update the STEP top cases for the extra row.
5. Produce JLCPCB Gerbers/BOM/CPL and a prototype build.
