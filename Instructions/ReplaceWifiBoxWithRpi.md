# Replacing the Built-In Wifi Box with a Raspberry Pi

Instructions for removing and replacing the built-in Wifi Box in the CR-10 Smart with a Raspberry Pi, including wiring instructions.

## Required Components
- Raspberry Pi
- Raspberry Pi Case (I used [this one](https://amzn.to/3jhbPnE))
- Short USB Micro B to USB A cable
- (optional) [Short Panel-mount USB A Female to USB A Male cable](https://amzn.to/3FA9JH8)

## Instructions
1. Power off and unplug the printer.
2. Wind the Z axis almost all the way to the top. Stop before the belt that connects both Z screws has slack in it.
3. Turn the printer on its side.
4. Use the included 2.0mm hex wrench to remove the 1 bajillion screws holding the bottom plate on the printer.
   - The two screws at the back and the one at the front-center are short while the rest are long.
   - There is a fan attached to the bottom plate which is plugged in to the mainboard. You may need to remove some hot-snot to unplug the fan and free the plate.
5. Unplug all cables from the Wifi Box board and re-route and remove some:
   - **NOTE**: There will be some hot snot on several of the connectors - make sure you remove it before lifting the connectors.
   - Remove the 2-wire cable from the mainboard. We will not use this cable.
   - Tuck the antenna cables (the ones with the tiny SMA connectors) away. We will not use these cables for now.
   - There is a cable running from the mainboard to the Distribution Board. In my printer, this was labelled PCB on both ends. On the Distribution board, move this connector from the socket labelled MCU to the socket labelled PC.
   - Remove the 4-pin cable that is connected to the Distrubtion Board socket labelled Wifi. We will modify this cable to provide power to the Raspberry Pi.
6. Use the included 2.0mm hex wrench to remove the 3 short screws holding the Wifi Box board to the chassis and remove the board.
   - My Wifi Box board had the model identification sticker on it. If yours does too, I recommend moving this to another location in the chassis.
7. Install your Raspberry Pi in some sort of case for mounting.
   - I modified my case by only using the bottom part and super-gluing some small but strong neodymium magnets to the bottom.
8. Using an 8GB or larger microSD card and the [Raspberry Pi Imager](https://www.raspberrypi.com/software/), flash the Mainsail OS image (under Other specific-purpose OS > 3D printing > Mainsail OS).
   - Since I am using a Raspberry Pi 3B+ which has a 64-bit processor, I chose the rpi64 variant.
9. Install the microSD card in your Raspberry Pi.
10. Mount the Pi in the CR-10 chassis where the Wifi Box board used to be.
    - I did this with neodymium magnets.
11. Modify the 4-wire cable labelled Wifi to power the Raspberry Pi:
    - Looking at the [GPIO Pinout](https://pinout.xyz/), you can see that pins 2 and 4 are 5V and pin 6 is GND. We are going to modify the cable to use pins 2 and 6.
    - On one end of the cable, de-pin the white, green, and black wires. You will need to insert a tiny, flat tool to push back the retention clip for each pin.
    - Cut off the pins on the white and green wires. We will not use these.
    - Since they are un-marked, we will assume that the red wire is currently in pin slot 1. You will need to insert the black wire into pin slot 3.
12. Connect the Raspberry Pi:
    - Insert the modified end of the 4-pin cable into pins 2, 4, 6, and 8 of the GPIO header. The 5V (red) wire should be on pin 2 and the GND (black) wire should be on pin 6.
    - Connect the external Ethernet port cable to the Raspberry Pi RJ-45 port.
    - Connect the Micro B to A cable to the Micro USB connector on the Distribution Board and one of the USB ports on the Raspberry Pi.
    - Make sure the Camera and Display connectors on the Raspberry Pi are not covered in case you plan to use them.
    - Strain-releive and manage cables to the best of your ability.
    - (optional) Remove the existing panel-mount USB cable on the side of the printer and replace with the male-to-female extension cable. Plug this into one of the Raspberry Pi USB ports.
13. Before putting the bottom cover back on, verify that powering on the printer also powers on the Raspberry Pi.
    - At this point, see the Optional Steps below if you want to add a camera or touch screen.
14. Re-attach the bottom plate on the printer and turn it right-side-up.
15. Connect an Ethernet cable between the port on the side of the printer and your local network switch or router.

## Optional Steps
- [Install Raspberry Pi Camera](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/InstallRaspberryPiCamera.md)
- [Replace the Stock Touch Screen](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/ReplacingTheStockScreen.md)
