# maiconvert-io4-breakout-board

Break-out / bench-test board that sits inline between a SEGA IO4 (`CN3` + `CN9`)
and the cabinet harness. Fans every I/O out to labelled JST-XH connectors and
adds on-board test switches and RGB indicator LEDs.

The `J22` 12 V input is protected against reverse polarity (P-channel MOSFET `Q1`
as an ideal diode: drain to input, source to rail, gate to GND via `R17`), fused
(`F1`, 15 A SMD Nano2), and clamped against transients (`D5`, SMCJ15A unidirectional
TVS on the rail after `Q1`). No standing over-voltage protection — supplying the
correct voltage is on the user. See `bom/README_BOM_DigiKey.md` for the wiring
notes and the fuse/copper trade-off.

- PCB fabricated bare at JLCPCB (2-layer, 1.6 mm, 1 oz).
- Components hand-soldered, sourced from DigiKey — see `bom/`.
