# RaspberryPI5Desktop
Instructions for producing a working desktop system using the Raspberry Pi 5

# Section 1: Equipment
My system includes the following components: <br>
1) Raspberry Pi 5 8 GB RAM <br>
2) Geekworm P579-V2 Metal Case with Official Pi 5 Active Cooler for Raspberry Pi 5 <br>
3) Geekworm X1001 PCIe to M.2 HAT Key-M NVMe SSD PIP PCIe Peripheral Board for Raspberry Pi 5 <br>
4) SABRENT 512GB Rocket NVMe PCIe M.2 2242 DRAM-Less Low Power Internal High Performance SSD (SB-1342-512) <br>
5) ViewSonic VA1655 15.6 Inch 1080p Portable IPS Monitor <br>
6) Raspberry Pi Keyboard + USB cable <br>
7) Logitech M317 Wireless Mouse <br>
8) WiFi for Raspberry Pi - Antenna and Instructions Included - Plug and Play by Detroit DIY Electronics <br>
9) Samsung EVO Plus 128 GB MicroSD card with SD Adapter <br>
10) CanaKit 5A USB-C Power Supply with PD for the Raspberry Pi 5 <br>
11) Generic USB Power Supply for monitor <br>
12) Micro HDMI to HDMI cable <br>
<p>At the time of this writing, the total cost of the system is about $450.00</p>

# Section 2: Initial Operating System Installation

Using the appropriate tool at <a href=https://www.raspberrypi.com/software/>Raspberry Pi OS</a>, install Raspberry Pi OS to the MicroSD card. Do not preload any settings. This tends to cause issues with the setting of the system locale and the keyboard language (I have found that the keyboard gets stuck in UK mode, where some keys like @ and # function differently.

# Section 3: Equipment assembly

1. Install Sabrent NVME card in Geekworm X1001 M.2 HAT
2. Mount and attach Raspberry Pi Active Cooler to Raspberry Pi 5
3. Mount and attach M.2 HAT to Raspberry Pi 5
4. Mount Raspberry Pi 5 in Geekworm P579-V2 case
5. Insert SD card
6. Connect WiFi Adapter
7. Connect Keyboard
8. Connect Monitor
9. Connect Power Supplies
10. Connect Wireless Mouse

# Section 4: Initial Boot + Firmware Update + NVME Operating System Installation

1. Boot and login to the Raspberry Pi 5
2. sudo rpi-update
3. sudo reboot
4. Use the built-in imager utility to write the operating system to the NVME drive (do not preset settings)
5. sudo nano /boot/firmware/config.txt
6. Add dtparam=pciex1_gen=3 to the end of config.txt and save (CTRL+X + Y)
7. sudo reboot
8. sudo raspi-config
9. From Advanced Options, select Bootloader Version and set to Latest.
10. Select Finish
11. sudo reboot
12. Remove MicroSD card
13. In /etc/sudoers.d/ execute sudo mv 010_pi-nopasswd .010_pi-nopasswd (this requires entering your password to use sudo)

# Section 5: Enable encryption of home directory

1. sudo apt-get install ecryptfs-utils rsync lsof
2. sudo modprobe ecryptfs
3. Add ecryptfs to /etc/modules-load.d/modules.conf
4. Enable root account by executing sudo su and using passwd to set a root password
5. Reboot; at logon screen, do Control+Alt+F1 to switch to a TTY. Log in as root.
6. Execute ecryptfs-migrate-home -u <your username>
7. Exit root account (exit)
8. Login as yourself and follow directions
9. Reboot
10. Delete backup files if everything is working properly
11. Use ecryptfs-unwrap-passphrase to store passphrase in a safe location

# Section 6: Install special software as desired

The "Recommended Software" application can install and remove specific software packages. I generally select the ones that I want (including, for example, Mathematica) and deselect the ones I do not want. For all other software installation, I use the standard package manager.
