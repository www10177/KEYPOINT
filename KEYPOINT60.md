# KEYPOINT Programmer 60 variant

This branch converts KEYPOINT into a split 60%-class keyboard intended for programming.

The design goal is explicit: **normal ANSI punctuation must remain directly available on layer 0**.
Symbols that programmers use constantly are not hidden behind Fn.

## Layer 0

```text
Esc  1  2  3  4  5   |   6  7  8  9  0  -  =  Backspace
Tab  Q  W  E  R  T   |   Y  U  I  O  P  [  ]  \
Caps A  S  D  F  G   |   H  J  K  L  ;  '  Enter
Shft Z  X  C  V  B   |   N  M  ,  .  /  Shft

LCtrl LGUI LAlt Space | RAlt Fn RGUI RCtrl
`/~ is also a dedicated layer-0 key.
```

Therefore the following programming symbols are direct on layer 0:

```text
` ~
1 2 3 4 5 6 7 8 9 0
! @ # $ % ^ & * ( )
- _ = +
[ { ] } \ |
; : ' "
, < . > / ?
```

No `-`, `=`, bracket, quote, semicolon, slash, backslash, comma, period, or grave/tilde
key requires an Fn layer.

The Fn layer is reserved for things a normal 60% already moves off layer 0, such as
F1-F12, arrows, Delete, Bluetooth management, bootloader, and output switching.

## KEYPOINT-specific controls

The TrackPoint/A320 functionality remains available. Seven KEYPOINT-specific logical
positions are appended after the conventional typing surface:

1. dedicated Grave/Tilde
2. second Space
3. left mouse click
4. right mouse click
5. pointer scroll modifier
6. pointer arrow modifier
7. slow-pointer modifier

The pointing-device drivers have been updated to follow the new logical position
numbers, so their hard-coded position listeners still refer to those dedicated controls.

## Matrix design

The firmware still uses the existing 6x8 matrix per half. The revised logical map uses
unused matrix intersections rather than requiring more nRF52840 GPIO pins.

This is an architectural advantage of the existing KEYPOINT design: the MCU already
has enough row/column capacity for the additional switches.

## PCB status

**The physical PCB has NOT been modified yet.**

The public upstream repository does not contain the original KiCad/Altium schematic,
PCB source, Gerber, or BOM. The current branch changes firmware/layout metadata only.

To manufacture the programmer 60 version, a new PCB revision must still be created.
That PCB must physically add/reroute the switches required by the new 60%-class layout
and preserve:

- nRF52840
- TrackPoint
- A320 optical trackpad
- both encoders
- memory LCD
- USB
- battery/charging/power circuitry
- split wireless operation

The current upstream STEP cases are also for the original layout and will need a matching
mechanical revision.

## Next hardware pass

1. Reconstruct the left/right schematic from the published DTS pin assignments.
2. Lock the exact split ANSI-style switch placement shown above.
3. Create a KiCad PCB using the existing 6x8 matrix wiring.
4. Retain the existing TrackPoint/A320/display/encoder interfaces.
5. Revise the STEP top cases around the new switch layout.
6. Generate JLCPCB Gerbers, BOM, CPL, and prototype files.
