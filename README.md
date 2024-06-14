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
<p>At the time of this writing, the total value of the system is about $450.00</p>

# Section 2: Initial Operating System Installation

Using the appropriate tool at <a href=https://www.raspberrypi.com/software/>Raspberry Pi OS</a>, install Raspberry Pi OS to the MicroSD card. Note that at this time, confirm the following settings: <br>
1. Hostname
2. Username
3. Password
4. Wireless LAN

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
4. Use the built-in imager utility to write the operating system to the NVME drive (confirm the same settings)
5. sudo nano /boot/firmware/config.txt
6. Add dtparam=pciex1_gen=3 to the end of config.txt and save (CTRL+X + Y)
7. sudo reboot
8. sudo raspi-config
9. From Advanced Options, select Bootloader Version and set to Latest.
10. Select Finish
11. sudo reboot
12. Remove MicroSD card

# Section 5: Enable encryption of home directory

Please note that these instructions were modified from the instructions provided by Leigh McCulloch at his <A href="https://leighmcculloch.com/posts/ubuntu-encrypt-home-directory-with-gocryptfs/">website</A>

1. Boot and login to the Raspberry Pi 5
2. sudo apt-get install -y libpam-mount gocryptfs
3. sudo nano /etc/fuse.conf, uncomment user_allow_other and save (CTRL+X + Y)
4. sudo nano /etc/security/pam_mount.conf.xml, and before the last XML tag (replacing your username) and save:
```console
<volume
  user="yourusername"
  fstype="fuse"
  options="nodev,nosuid,quiet,nonempty,allow_other"
  path="/usr/local/bin/gocryptfs#/home/%(USER).cipher"
  mountpoint="/home/%(USER)"
/>
```
5. Backup your home directory:
```
cd /home
sudo tar cvf $USER.tar $USER
```
6. Create the encrypted directory:
```
sudo mkdir $USER.cipher
sudo chown $USER:$USER $USER.cipher
```
7. Initialize encryption: gocryptfs -init $USER.cipher
8. Clear the home directory: rm -fr /home/$USER/* /home/$USER/.*
9. Add a file to indicate if the encrypted directory is not mounted: touch /home/$USER/GOCRYPTFS_NOT_MOUNTED
10. Mount the encrypted directory: gocryptfs $USER.cipher $USER
11. Copy the home directory contents into the encrypted directory: tar xvf $USER.tar --strip-components=1 -C $USER
12. Add file to indicate if the encrypted directory is mounted: touch /home/$USER/GOCRYPTFS_MOUNTED
13. sudo reboot, check after login that GOCRYPTFS_MOUNTED is present in the home directory
14. Remove backup files
