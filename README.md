# Zeroing Offset Target Generator

Print a rifle zeroing target for a short range that sets up a longer zero. At 10 yards the bullet hasn't crossed the line of sight yet, so it prints low; aim at the diamond, put the group on the circle, and you're on the trajectory that zeroes at 100.

**Use it:** https://kharyam.github.io/zeroing-offset-targets/

Single HTML file. Open `index.html` in a browser — no install, no server. It pulls jsPDF from a CDN the first time and works offline after that. Inventory lives in localStorage; export/import as JSON.

## What it does

- Keeps an inventory of firearms: sight height, load (velocity + G1 BC), barrel length, zero distance, turret click value, and the distances you'll shoot at (10 and 25 yards by default — shoot the 10 first, dial, then the 25).
- Generates a print-exact PDF: cover page with instructions, then one target per firearm per distance, each labelled with the distance to hang it at. Full-page 0.25 in grid centered on the expected impact, two 4 in rulers to catch printer scaling, sight-height variance table, clicks-per-square note.
- Click calculator: measure the group from the impact circle, get turret clicks with direction.
- Printer calibration: measure the rulers once, enter what you got, and every PDF after that is compensated per axis and stamped.
- Refuses to print things that aren't zeroing targets — offsets that don't fit on paper, and long zeros where the drop model rather than sight geometry would be setting the mark.
- Side view of the trajectory, and a through-the-scope view with a holdover slider and magnification lever, for understanding what the numbers mean downrange. Pick a reticle (fine hash, duplex, mil-dot, MOA ladder, TREMOR3-style tree, ACSS-style BDC), FFP or SFP; BDC reticles are checked against your load.

## Math

G1 point-mass integrator, flat fire, standard atmosphere. The offset relation is

```
offset = SH − (d / Z) × (SH + drop(Z)) + drop(d)
```

Positive is impact below aim. A self-test runs on every load against the golden vectors in the spec (chip in the top bar).

## Spec

`zero-target-app-spec.pdf` is the build specification the app was written to. Its prose examples aren't all right; the golden vectors and the reference drawing code are.

## Caveats

No wind, spin drift, or angle of fire. This is a first-shot-on-paper zero; confirm at the real distance.
