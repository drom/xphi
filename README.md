# xphi
Xeon Phi board bring up

## Hardware

![PCB Top](img/5120d_top.jpg)
### Setup
 - Intel® Xeon Phi™ Coprocessor 5120D [Intel Xeon Phi Datasheet](http://www.intel.com/content/dam/www/public/us/en/documents/datasheets/xeon-phi-coprocessor-datasheet.pdf)
 - Z97 Motherborad ???
 - PCIe x16 to x24 converter (todo)

### PCIe x16 to x24 converter

Phi board comes with x24 edge connector, that is not compatible with ANY reasonable (available) Motherboard.
The x24 connector using only 16 of PCIe lanes, and uses ther rest of pads for +12V power delivery.

So here is our plan:

 - [PCIe x16 extention cable](http://amzn.com/B00D79EV0G) $7
 - PCIe x24 connector (STRADDLE MOUNT) ~$12
 - Power cable modular PSU 8-pin (+12v) GPU connector?
 - Soldering

## Software
 - TBD

