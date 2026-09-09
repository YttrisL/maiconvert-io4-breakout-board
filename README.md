# maiconvert-io4-breakout-board

Break-out board designed to sit inline next to a SEGA IO4 (`CN3` + `CN9`) and the cabinet harness. Fans every I/O out to labelled JST-XH connectors and adds on-board test switches and RGB indicator LEDs. Ideal for an IO4 test setup or for an installation outside an official cab.

<p align="center">
  <a href="images/schematic.jpg"><img src="images/schematic.jpg" width="32%" alt="Schematic"></a>
  <a href="images/pcb-front.jpg"><img src="images/pcb-front.jpg" width="32%" alt="3D render — front"></a>
  <a href="images/pcb-back.jpg"><img src="images/pcb-back.jpg" width="32%" alt="3D render — back"></a>
</p>
<p align="center"><sub>Schematic &nbsp;·&nbsp; PCB 3D render (front) &nbsp;·&nbsp; PCB 3D render (back) — click any image for the full-size version</sub></p>

You need to connect your own 12V line to the board, the `J22` 12 V input is protected against reverse polarity (P-channel MOSFET `Q1` as an ideal diode: drain to input, source to rail, gate to GND via `R17`), fused (`F1`, 15 A SMD Nano2), and clamped against transients (`D5`, SMCJ15A unidirectional TVS on the rail after `Q1`). Be aware though that it'll preserve your components but it will still blow the fuse, be careful of the polarity when plugging your 12V cables.
There is no standing over-voltage protection, please make sure you are connecting a 12v line.

Do not re-use the 12V from the IO4, it is isolated for a reason. In actual maimai DX cabinets there is a PSU dedicated for the LEDs 12v rail. Use that if installing in a cab, or use your own dedicated PSU.

- The PCB is designed to be fabricated bare at JLCPCB (2-layer, 1.6 mm, 1 oz). See gerbers in `production/`
- Components are hand-soldered, because most of them are THT and JLCPCB charges a fee for those. See `bom/`, the list is made for DigiKey and can be directly uploaded on the website.
- To fab the PCB: on [jlcpcb.com](https://jlcpcb.com/) click *Add gerber file* and upload `production/maiconv-io4-breakout-board-gerbers.zip` as-is. The auto-detected options already match this board (2 layers, 1.6 mm, 1 oz copper) — just choose a solder mask colour, leave *PCB Assembly* off (all THT), check the Gerber preview, and order.
- To order the parts: sign in to [digikey.com](https://www.digikey.com/), go to *myLists* → *Create a New List* → *Upload a File*, and upload `bom/digikey_upload.csv`. Map the columns to *Quantity* / *DigiKey Part Number* / *Customer Reference* (the header already uses those names), then review any out-of-stock lines before adding the list to your cart.

Please report any issue encountered.
