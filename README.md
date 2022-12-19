# CR10SmartOctoWRT

This repository contains instructions and relevant informmation about the Creality CR-10 Smart and modifying the built-in Creality Wifi Box to run OctoWRT instead of the built-in firmware.

**CAUTION** The information provided in this repository is for educational purposes only. If you brick your printer, it's not my fault.

## Instructions

1. Power off and unplug the printer.
2. Wind the Z axis almost all the way to the top. Stop before the belt that connects both Z screws has slack in it.
3. Turn the printer on its side.
4. Use the included 2.0mm hex wrench to remove the 1 bajillion screws holding the bottom plate on the printer.
   - The two screws at the back and the one at the front-center are short while the rest are long.
   - There is a fan attached to the bottom plate which is plugged in to the mainboard. You may need to remove some hot-snot to unplug the fan and free the plate.
5. Remove the MicroSD card from the Wifi board.
6. Go to https://github.com/ihrapsa/KlipperWrt/tree/main/Firmware/OpenWrt_snapshot and download the latest `*-factory.bin` file.
7. Put the MicroSD card from step 5 into your PC.
8. Make a backup of the stock firmware on the card (either by creating an ISO image or copy/paste-ing all of the files to your local machine).
9. Rename the download from step 6 to `cxsw_update.tar.bz2` and copy it to the MicroSD card.
10. Properly eject the MicroSD card from your PC.
11. Put the MicroSD card back into the Wifi board.
12. Set the printer upright and connect it via Ethernet to your PC.
13. Plug in and power on the printer.
14. Wait for the wifi network OpenWRT to appear and connect to it.
    - Optionally, if you want to connect it to your existing network, you can connect the Ethernet port to your router or switch.
15. Once connected, open a browser and navigate to http://192.168.1.1:81/cgi-bin/luci/.
    - *Note*: If you connected via Ethernet to your existing network, you'll have to check your DHCP server for a new client to find the IP address and use it in the URL above.
    - Log in with the username `root` and no password.
    - Make sure to set a new password under System > Administration once logged in.
16. Set a static IP address for the printer under Network > Interfaces.
    - On the version of OpenWRT that I downloaded, this involved editing the `br-lan` interface and setting a static IP in the subnet of my home network (outside the DHCP address range).
    - If you want the printer to connect to your Wifi, you'll need to add a wireless connection under Network > Wireless. Once you've configured the network, click Save & Apply.
    - Once you are able to connect to the printer through your home network, remove the "OpenWRT" network under Network > Wireless.
    - Don't forget to set a gateway (your network's) and DNS servers for the interface.
17. From here, follow the instructions at https://damsteen.nl/blog/2022/02/10/how-to-install-octowrt-on-creality-wifi-box#connecting-to-a-command-prompt. When you get to the section "Installing Octoprint", come back here.
    - **NOTE**: Install version 1.8.2 *or earlier*. 1.8.6 contains dependencies on packages that are not available on OpenWRT.
18. Several instructions in the "Installing Octoprint" section of the link from 17 were broken or no longer worked. Follow the below instructions instead.
19. Update the system and install some dependencies.
```
opkg update
opkg install gcc make unzip htop wget-ssl git-http
opkg install v4l-utils mjpg-streamer-input-uvc mjpg-streamer-output-http mjpg-streamer-www
```
20. Install Python3 and dependencies.
```
opkg install python3 python3-pip python3-dev python3-psutil python3-pillow
wget -O /tmp/python3.9-regex-2022.1.18%2Bnetifaces-0.11.0_mipsel_24kc.ipk https://github.com/ihrapsa/OctoWrt/raw/main/packages/python3.9-regex-2022.1.18%2Bnetifaces-0.11.0_mipsel_24kc.ipk
opkg install /tmp/python3.9-regex-2022.1.18%2Bnetifaces-0.11.0_mipsel_24kc.ipk
```
21. Install more Python dependencies (these will take a while).
    - **NOTE**: `opkg` installs a "system pip" and you should, therefore, ignore the message regarding upgrading `pip`. Doing so can cause your system to become unrecoverable.
```
pip install --upgrade setuptools
pip install wheel
pip install frozendict
pip install immutabledict
```
22. Install Octoprint. This will take a *VERY* long time. Be patient.
    - **NOTE**: DO NOT install any version of OctoPrint above 1.8.2. Version 1.8.3 added a dependency on `passlib` which is not available on OpenWRT.
    - If you want, you can connect another SSH session and use `htop` to make sure the system is still doing something.
```
pip install octoprint==1.8.2
```
23. Start Octoprint on system boot:
```
cat << "EOF" > /etc/init.d/octoprint
#!/bin/sh /etc/rc.common
# Copyright (C) 2009-2014 OpenWrt.org
# Put this inside /etc/init.d/

START=91
STOP=10
USE_PROCD=1

start_service() {
    procd_open_instance
    procd_set_param command octoprint serve --iknowwhatimdoing
    procd_set_param respawn
    procd_set_param stdout 1
    procd_set_param stderr 1
    procd_close_instance
}
EOF
chmod +x /etc/init.d/octoprint
service octoprint enable
```
23. Reboot with the command `reboot`.
24. Connect to OctoPrint in a browser at http://<ip_address>:5000/.
    - First startup will take 5-10 minutes before the OctoPrint UI is available (after boot).
