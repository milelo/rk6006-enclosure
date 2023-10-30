# USB-C powered RK6006-module enclosure

Design using RealThunders [Link Branch Release] of FreeCAD and the Sheetmetal workbench.

Note: this isn't compatible with the standard FreeCAD release.

Laser cutting file with 0.001mm line thicknesses: [rk6006 enclosure cuts pdf]. This can be cut from a single piece of 300x200mm cast-acrylic (Perspex®).

## Assembled unit

![assembly](usb-powered-psu.jpg "Assembled unit")
This is the fully assembled unit powered-up. Note there is a short across the output terminals.

<!--
This doesn't render because of 0.001mm line thicknesses.
It also doesn't render as generated direct from FreeCAD export because of 100% line stroke thickness.
I haven't found a way to render PDF inline.
<img src="rk6006-enclosure-cuts.svg" width="600" title="rk6006 enclosure cuts">
-->
## Features

* The [RK6006] needs a minimum supply voltage of 12V with which its maximum output voltage will be 9.9V (^Vo = Vi / 1.1 - 1).
* Typical, reasonably powered PD chargers can output 20V giving a maximum output of 17V.
* The [RK6006] maximum output current is 6A. This is not limited by the maximum current output of the PD charger current rather the chargers **power** rating. So a 67W charger should be able to supply 6A up to a maximum voltage of ~11V (67W/6A) above which the maximum current will trail off to ~3.3A (67W/20V). Note the conversion efficiency of the [RK6006] isn't 100% so the actual values will be slightly less than this.
* The Power Delivery Decoy module I have used to power the RK6006 should support the 3.1 standard supporting charger voltages of 28V and 48V and 5A with a suitable USB-C cable. I don't currently have anything to test this with. The module selects the maximum power configuration of your PD Power adaptor.
* Standard USB-C cables should support 3A.
* 
* More to come....

## Components

1. RIDEN® RK6006/RK6006-BT 60V 6A Power Supply Buck Converter.
1. USB-C PD3.1 Decoy Module - negotiates the maximum voltage and current output from a PD Power adaptor to supply the RK6006.
1. Red + Black 4mm Binding Post 10A
1. 3mm Perspex 300x200mm
1. DC 5V 2Pin Cooling Fan 40x40x10mm
1. 12 x M3 Coarse (0.5mm) 15mm black nylon pan-head screws
1. 4 x M3 nylon washers - optional for the lid screws to cover the lid slots.
1. Connecting wire. I have used insulated solid copper wire 1.5mm^2 for the input and 2.5mm^2 for the output from some house ring-main cable I had. This could be considered an over-kill and isn't critical however thinner the wire the more power loss and volt drop across the cable which could show up in the RK6006 meter readings at higher currents I'm not sure how significant the volt-drop is compared to internal volt-drops in the unit.
1. USB-C power adaptor - Potentially any that can output at least 12V. I have a fairly cheap but very compact and efficient 67W GAN-III unit - see the [BOM]

For component details and potential suppliers see the [BOM]

## Required Tools

1. Laser cutter for the 3mm cast-acrylic
1. Acrylic line-bend heater - I made one with some nichrome wire and this power supply; more information to come.
1. Two perpendicular surfaces to set the 90deg bends against, preferably with a sharp inside radius at the butt off the surfaces. I used a metal book-end.
1. Taper tap - M3 Coarse (0.5mm) - Required to cut the screw threads in the acrylic laser cut holes.

## Bending guidelines



## To-do

 1. Add all the details to this page.
 1. The USB Module clip has been modified to secure its vertical position. The base unfold doesn't recalculate and needs to be regenerated to incorporate this.
 1. The USB module mount has been reworked. Ideally the usb-chock part should have a location lug through the back panel.
 1. Provide details of the line bender and a line bending tutorial.

[RK6006]: https://www.aliexpress.com/item/1005005429587089.html
[Link Branch Release]: https://github.com/realthunder/FreeCAD/releases
[rk6006 enclosure cuts pdf]: rk6006-enclosure-cuts.pdf
[BOM]: https://docs.google.com/spreadsheets/d/1T6NbnWi5dBHlQWhf0BndDVR0SKQtk_TcdFyvAGypmZ0/edit?usp=sharing
[RK6006 user manual]: http://www.ruidengkeji.com/inst/RK6006.pdf
[PD3.1 USB-C Decoy module]: https://www.aliexpress.com/item/1005005881131564.html
