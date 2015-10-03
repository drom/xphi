# xphi
Xeon Phi board bring up

## Hardware

![PCB Top](img/5120d_top.jpg)
### Setup
 - Intel® Xeon Phi™ Coprocessor 5120D [Intel Xeon Phi Datasheet](http://www.intel.com/content/dam/www/public/us/en/documents/datasheets/xeon-phi-coprocessor-datasheet.pdf)
 - Z97 mini-ITX Motherborad ???
 - PCIe x16 to x24 converter (todo)

### Thermal solution
81 x 85 mm mount holes (3.8mm) diameter.

 - 200W cooler for the Phi.
 - top GDDR5 radiator x 4 sides
 - back GDDR5 radiator x 4 sides

### PCIe x16 to x24 converter

Phi board comes with x24 edge connector, that is not compatible with ANY reasonable (available) Motherboard.
The x24 connector using only 16 of PCIe lanes, and uses ther rest of pads for +12V power delivery.

Our plan is to modify [PCIe x16 extention cable](http://amzn.com/B00D79EV0G) $7

#### Option 1
Rework the converter board with PCIe x24 230 positions connector (STRADDLE MOUNT) ~$12

#### Option 2
Trim x16 connector and and one more PCIe connector (x8?) and glue them together.
 
#### Common
Both options require power cable modular PSU 8-pin (+12v) GPU connector?

## Software
 - TBD

