# CR10SmartUpgrades

This repository contains instructions and relevant informmation about the Creality CR-10 Smart and applying usability and functionality upgrades.

**CAUTION** The information provided in this repository is for educational purposes only. If you brick your printer, it's not my fault.

## Notes

- These instructions include affiliate links to products on Amazon. If you choose to purchase these products through the links, I get a commission.
- While it may be technically possible to re-flash the STM32F103 that is used on the CR-FDMv2.5.S1_100 mainboard to use something like Klipper, it is mechanically in-feasible (trust me, I tried) as it requires providing 3.3V on start-up to the BOOT0 pin of the chip (which is not brought out to any pads or traces) and not touching any of the nearby pins. Flashing a non-stock firmware from the SD card always failed on my printer.

## Instructions

- [Installing OpenWRT and OctoPrint on the built-in Wifi Box](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/WifiBoxToOpenWRT.md)
  - **NOTE**: This is VERY slow. I recommend replacing with a Raspberry Pi
- [Replacing built-in Wifi Box with Raspberry Pi](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/ReplaceWifiBoxWithRpi.md)
  - [Adding a Raspberry Pi Camera](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/InstallRaspberryPiCamera.md)
  - [Replacing the Stock Touch Screen](https://github.com/JWhitleyWork/CR10SmartUpgrades/blob/main/Instructions/ReplacingTheStockScreen.md)
