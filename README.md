# USB-C powered RK6006-module enclosure

Design using RealThunders [Link Branch Release] of FreeCAD and the Sheetmetal workbench.

Note: this isn't compatible with the standard FreeCAD release.

Laser cutting file with 0.001mm line thicknesses: [rk6006 enclosure cuts pdf]. This can be cut from a single piece of 3mm 300x200mm cast-acrylic (Perspex®).

## Assembled unit

![assembly](usb-powered-psu.jpg "Assembled unit")
This is the fully assembled unit powered-up. Note there is a short across the output terminals.

<!--
This doesn't render because of 0.001mm line thicknesses.
It also doesn't render as generated direct from FreeCAD export because of 100% line stroke thickness.
I haven't found a way to render PDF inline.
<img src="rk6006-enclosure-cuts.svg" width="600" title="rk6006 enclosure cuts">
-->

## Key Features

* The [RK6006] needs a minimum supply voltage of 12V with which its maximum output voltage will be **9.9V** (from the RK6006 specification: `^Vout = Vin / 1.1 - 1`). In reality, it will be slightly higher.
* Typical, reasonably powered PD adaptors can supply 20V giving an output of **~18.5V** (17.1V from the specification).
The [RK6006] displays its input voltage (VIN), see the photo.
* The [RK6006] maximum output current is 6A. This is **not** limited by the maximum current output of the PD charger, rather the chargers **power rating**. This is a characteristic of the Buck Converter. See the "Maximum output power section below". A 67W charger should be able to provide 6A up to an output voltage of ~10.4V.
* The [PD3.1] Power-Delivery Decoy module I use to feed the RK6006 selects the maximum power configuration of your PD Power adaptor. The module implements the PD3.1 standard, supporting higher power with charger voltages of 28V and possibly 36V and 48V (to be confirmed). Also 5A current with a suitable USB-C cable. I don't currently have anything to test this with.
* Standard USB-C cables support 3A. Some none-standard adaptors can exceed this (eg 3.35A) and are typically supplied with a higher rated USB-C cable although nothing stops them from being used with a 3A cable.
* USB-C cables for use with 5A supplies contain an [E-Mark] chip to negotiate their capability unless the supply has a built-in cable.
* The Fan isn't enabled while the output current < 3.9A and System temperature < 40℃. The RK6006 displays its system temperature. I've run the unit with continuous 6A output at lower voltages and the unit remained cool. If you want to test the fan just ensure the output if off with an output current limit of > 4A, short the output terminals and switch the unit on.

## Maximum output power

* From the RK6006 specification the its maximum output voltage is `^Vo = Vi / 1.1 - 1`. For a 20V adaptor this is `20V/1.1 - 1 = 17.1V`. In reality it may be higher under low load conditions. I've measured 18.7V.
* Although the RK6006 can potentially draw more current from an adaptor than the adaptor is rated at, to support USB 3.0 specification the PD devices must implement an "over-current limiting mechanism" that is "resettable without user mechanical intervention" [^over-current-protection]. In reality power adaptors will typically supply power beyond their maximum rating.
* If the PD adaptor cuts out, the RK6006 will temporarily lose power and will restart with its output in the off-state.
* The RK6006 can be configured to limit its output power to that of a particular DP adaptor however unless you always use it with the same adaptor I've found this to be more cumbersome than helpful. Also if the output terminals are shorted with the output on (as opposed to shorting the output terminals and then switching the output on) then the current surge could still trip the PD protection before the RK6006 can react to enforce its current-limit.
* From the tables below the difference between the adaptor rated power and the listed maximum output power is the power dissipated by the RK6006 module at the rated load. For example for the 67W adaptor it is **~4.5W** (`67W-62.5W`).

I'd normally expect USB adaptors to adequately protect themselves given the USB specification states they should, however be aware, running an adaptor beyond its rated power output could reduce it's lifespan. See the tables below for the maximum output power you can draw to keep within the adaptors rating.

[^over-current-protection]: [For over-current protection, USB 3.0 Specification Section]
11.4.1.1.1 states:
The host and all self-powered hubs must implement over-current protection for safety reasons, and the hub must have a way to detect the over-current condition and report it to the USB software.
The over-current limiting mechanism must be resettable without user mechanical intervention. Polymeric PTCs and solid-state switches are examples of methods that can be used for over-current limiting.

### Adaptors supporting 20V Power Delivery

* Typical, reasonably powered PD adaptors can supply 20V giving a maximum specified output voltage of 17.1V (`20 / 1.1 - 1`). In reality I have found the output to be higher at **18.5V** to 18.7V.
* This reduced output voltage is due to losses in the RK6006 that also reduces the output power that can be supplied by the RK6006. For a 20V charger multiply the adaptor power rating by **0.93** `(18.6V/20V)` to get the maximum output power.
* The maximum output current of the RK6006 is 6A. This can be supplied up to an output voltage that doesn't cause the adaptors power rating to be exceeded (for 67W adaptor `62.3W/6 = 10.4V`) above which maximum current will trail off to the maximum rated adaptor current (3.35A for a 67W adaptor `67W/20V`). See the table.

| Adaptor Power | Displayed </br> output power<sup>[1]</sup> | 6A ^V<sup>[2]</sup> | Displayed </br> output power </br> cut-out threshold<sup>[3]</sup> | Note
|---|---|---|---|---|
| 100W | 93.3W | 14.3V || requires a built-in or 5A [E-Mark] USB-C cable
| 90W | 83.9W | 14.0V || requires a built-in or 5A [E-Mark] USB-C cable
| 67W | 62.5W | 10.4V |
| 65W | 60.3W | 10.0V |
| 60W | 55.9W | 9.3V |
| 45W | 42.0W | 7.0V |
| 30W | 28.0W | 4.7V | 31.3W |

* 1 The maximum output power as displayed on the RK6006 to keep within the adaptors power rating.
* 2 The maximum voltage at which 6A output current can be supplied.
* 3 Measured cut-out output power from a sample adaptor. Note: this is the supplied power, the power drawn from the adaptor is higher.

## Project Build

### Components

1. RIDEN® RK6006/RK6006-BT 60V 6A Power Supply Buck Converter. Here is the [RK6006 user manual] and a good review: [Kerry Wong RK6006 review].
1. USB-C [PD3.1] Decoy Module - negotiates the maximum voltage and current output from a PD Power adaptor to supply the RK6006.
1. Red + Black 4mm Binding Post 10A
1. 3mm Perspex 300x200mm
1. DC 5V 2Pin Cooling Fan 40x40x10mm
1. 12 x M3 Coarse (0.5mm) 15mm black nylon pan-head screws - nylon screws can be easily cut to size with side cutters.
1. 4 x M3 nylon washers - optional for the lid screws to cover the lid slots.
1. Connecting wire. I have used insulated solid copper wire 1mm² for the input and 2.5mm² for the output from some house ring-main cable I had. This could be considered an over-kill and isn't critical however thinner the wire the more power loss and volt drop across the cable which could show up in the RK6006 meter readings at higher currents I'm not sure how significant the volt-drop is compared to internal volt-drops in the unit.
1. USB-C power adaptor - Potentially any that can output at least 12V. I have a fairly inexpensive but compact and efficient 67W GAN-III unit - see the [BOM]

For component details and potential suppliers see the [BOM]

### Required Tools

1. Laser cutter for the 3mm cast-acrylic
1. Acrylic line-bend heater - I made one with some nichrome wire and this power supply; more information to come.
1. 90deg jig - Two perpendicular surfaces to set the 90deg bends against, preferably with a sharp inside radius at the butt off the surfaces. I used a metal book-end.
1. Taper tap - M3 Coarse (0.5mm) - Required to cut the screw threads in the acrylic laser cut holes.
1. Soldering iron and solder to connect the PD Decoy module and output terminals.

### Bending guidelines

1. I've used a bend radii in the design that I've found the heated acrylic naturally bends to, without being forced. The line heated acrylic can be picked up and bent to roughly 90deg then placed in the 90deg jig to set the bend angle.
1. For some of the bends the un-bent parts of the material that fall on the bend-line will need protecting from the heat. If protection is only required on one side then the hot wire ends can be used. If both, either a heat shield will be required or the length of the heated wire reduced with a current shunt. see below
1. ToDo...

#### heat shield

#### current shunt

## Wiring the module

1. Note: the RK6006 terminal block can be unplugged from the unit, this greatly helps accessibility.
1. ToDo...

## To-do

 1. Provide details of the line bender and a line bending tutorial.

[RK6006]: https://www.aliexpress.com/item/1005005429587089.html
[Link Branch Release]: https://github.com/realthunder/FreeCAD/releases
[rk6006 enclosure cuts pdf]: rk6006-enclosure-cuts.pdf
[BOM]: https://docs.google.com/spreadsheets/d/1T6NbnWi5dBHlQWhf0BndDVR0SKQtk_TcdFyvAGypmZ0/edit?usp=sharing
[RK6006 user manual]: http://www.ruidengkeji.com/inst/RK6006.pdf
[PD3.1 USB-C Decoy module]: https://www.aliexpress.com/item/1005005881131564.html
[E-Mark]: https://satechi.net/blogs/identifying-usb-c-e-mark-cables
[PD3.1]: https://www.graniteriverlabs.com/en-us/technical-blog/usb-power-delivery-specification-3-1
[For over-current protection, USB 3.0 Specification Section]: https://www.littelfuse.com/~/media/electronics_technical/application_notes/usb/littelfuse_usb_3_0_circuit_protection_application_note.pdf 
[Kerry Wong RK6006 review]: https://youtu.be/FeJ0dfOLvTA

---

This work is Copyright © 2023 Mike Longworth

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
