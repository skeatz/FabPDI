## FabPDI-t90usb
The FabPDI-t90usb programmer is a PDI/ISP programmer, based on Atmel's [AVRISP-MkII](http://www.atmel.com/tools/avrispmkii.aspx). The main features of this board are:
* based on Atmel's AVRISP-MkII
* uses the AT90usb162 microcontroller
* [LUFA library](http://www.fourwalledcubicle.com/LUFA.php) by Dean Camera
* operates as both PDI & ISP programmer

You could substitute the AT90usb162 by other members of the AT90usb family or the ATmega16U2 or higher devices, as long as it has > 16 kB memory and hardware USB support.

Since the FabPDI-t90usb board is a clone of the AVRISP-MkII, it will work directly with standard versions of AVRDUDE without any need to patch the software.

There are 2 steps involved in building the FabPDI-t90usb programmer:
1. Mill and stuff the PCB
2. Upload firmware to the board

My main references for this project were:
* [USBtiny-mkII by mdiy](http://mdiy.pl/programator-usbtiny-mkii-slim/?lang=en)
* [LUFA library & Projects](http://www.fourwalledcubicle.com/AVRISP.php)

## FabPDI-t90usb Schematic & PCB Layout
The AVRISP-MkII clone project at [fourwalledcubicle](http://www.fourwalledcubicle.com/AVRISP.php) supports PDI, ISP and TPI programming. Since I was only interested in implementing the PDI and ISP protocols, I left out the TPI programming interface, to reduce the dimensions of the PCB.

![FabPDI-t90usb Schematic](images/fabpdi-t90usb_01.png)

*FabPDI-t90usb schematic diagram*

![FabPDI-t90usb PCB layout](images/fabpdi-t90usb_02.png)

*FabPDI-t90usb pcb layout*

**Note** the use of a jumper wire at the top left of the pcb layout diagram. There are 2 other 0 ohm resistors in my design. I opted to use a jumper wire to save time, instead of trying to changing the component placement and re-route the entire board.

**Tip**: when I started laying out the PCB, I did not know if I needed any 0 ohm resistors or where they should be located. To add a 0 ohm resistor in Eagle board layout, you need to de-link the PCB editor from the Schematic editor. You can do this by closing the Schematic editor. This allows you to add components in the PCB editor, but you lose the ability to do forward or backward annotation.

Fab modules downloads (1269dpi):

* [FabPDI-t90usb board outline](images/fabpdi-t90_outline.png)
* [FabPDI-t90usb pcb trace](images/fabpdi-t90_traces.png)

### Mill and stuff the pcb.

Most of the traces are 15 mil wide, except for the traces connected to the [GTL2003-PW](http://www.nxp.com/docs/en/data-sheet/GTL2003.pdf) voltage level translator. The pins on this device are very fine and close together, so care must be taken in soldering this chip.

![FabPDI-t90usb completed board](images/fabpdi-t90usb_03.png)

*Completed FabPDI-t90usb board*

### Testing the board
After milling & stuffing the FabPDI-t90usb, connect it to a USB port. In Windows, check *Device Manager* to verify that the board is correctly detected by the system. In Windows, the FabPDI-t90usb identifies itself as an AVRISP mkII. In Linux, enter the command ***lsusb*** in a terminal screen. You should see a new device called ***at90usb162 DFU***.

![FabPDI-t90usb in Windows Device Manager](images/fabpdi-t90usb_04.png)

![FabPDI-t90usb in Linux](images/fabpdi-t90usb_05.png)

## Component List
| Components | Components |
| :--------- | :--------- |
| 1 x AT90usb162 | 1 x Mini-USB |
| 1 x GTL2003PW | 2 x PinHD 2x3 ISP |
| 1 x AMS1117-3.3 regulator | 2 x PinHD 1x2 HW, RST |
| 1 x 16 MHz crystal | 1 x PinHD 1x3 |
| 1 x LED red | 1 x LED green |
| 1 x 10uF capacitor | 1 x 1uF capacitor |
| 3 x 0.1uF capacitor | 2 x 22pF capacitor |
| 1 x 100k ohm R | 1 x 10k ohm R |
| 3 x 1k ohm R |  4 x 49 ohm R |

### Update
I have since revised my PCB layout to make it a bit smaller and easier to mill on our Fablab standard 3" x 2" copper blanks. I did this by shifting the placement of some of the components and changing the location of one of the 0 ohm resistors. The revised schematic & pcb layout are below.

![FabPDI-t90usb revised schematic](images/fabpdi-t90usb_01a.png)

*Revised schematic diagram*

![FabPDI-t90usb revised PCB layout](images/fabpdi-t90usb_02a.png)

*Revised pcb layout*

Fab modules download (1269dpi):
* [FabPDI-t90usb board outline](images/fabpdi-t90usb_outline.png)
* [FabPDI-t90usb pcb trace](images/fabpdi-t90usb_traces.png)

## Programming the firmware
### For Windows
The FabPDI-t90usb is a DFU class device and can be programmed via the DFU protocol. Download and install the Atmel [FLIP](http://www.atmel.com/tools/flip.aspx) software. The software requires Java Runtime Environment. 2 versions of the software are available at the Atmel site - with and without JRE. I opted to install the version with JRE, as the installation would then take care of all dependencies and environment variables required.

After the software is installed, install the DFU drivers from the \Program Files\Atmel\Flip 3.4.7\usb folder. Start FLIP, click on the **"CHIP"** icon and select at90usb162 from the list. Then click on the **"usb cable"** icon, select **USB** and **OPEN**. FLIP should detect the programmer.

To program the board, jumper the **HWB** pins. Short the **RST** pins momentarily. The board will start in bootloader mode. If the AT90usb162 is brand new (empty), it will automatically start in bootloader mode.

Download the pre-compiled [firmware](files/fabpdi-t90usb/fabpdi-t90usb_firmware.hex). Click on the **"Open Book"** and load the firmware. In the left column of the FLIP window, check the **"Erase"**, **"Program"** and **"Verify"** checkboxes. Click the **RUN** button. After the firmware has been uploaded to the board, disconnect and reconnect it. Alternatively, just short the **RST** pins. The two LEDs should light up and then the red LED will remain ON. The FabPDI-t90usb is ready for use.

Congratulations!

### For Linux

## Files
* [Eagle FabPDI-t90usb schematic (v1)](files/fabpdi-t90usb/fabpdi-t90usb_v1.sch)
* [Eagle FabPDI-t90usb pcb layout (v1)](files/fabpdi-t90usb_v1.brd)
* [Eagle FabPDI-t90usb schematic (v1.1)](files/fabpdi-t90usb/fabpdi-t90usb_v1.1.sch)
* [Eagle FabPDI-t90usb pcb layout (v1.1)](files/fabpdi-t90usb/fabpdi-t90usb_v1.1.brd)

*Copyright (c) 2017 Steven Chew*

*MIT license*
