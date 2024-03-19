# rpi-usb-drive
Script for setting up a Raspberry Pi as a USB flash drive

Note: this will only work on a Raspberry Pi that supports OTG. On the Pi 4B the USB-C port serves as both the OTG port and the power port. This was quite annoying since I had to supply power through the 5V/GND pins on the GPIO header instead. 

Steps to setup: 
1. Add `modules-load=dwc2` after `rootwait` in the `/boot/firmware/cmdline.txt` file. (Note: older versions of the OS may have this as the `/boot/cmdline.txt` file).
2. Add `dtoverlay=dwc2` on a new line after `[all]` in the `/boot/firmware/config.txt` file. (Note: older versions of the OS may have this as the `/boot/config.txt` file).
3. Create a file to act as the backing store: `dd if=/dev/zero of=/usbstore.img bs=1M count=1K` (this is a 1GB file).
4. Format the backing store:
    1. Attach the backing store to a loop device - `sudo losetup --show -fP /usbstore.img`
    2. Initialise, partition and format to the FAT32 filesystem - `sudo mkfs.vfat -F 32 /dev/loop0` (or replace `/dev/loop0` with the output from the previous step, if not identical)
    3. Detach the backing store from the loop device - `sudo losetup -d /usbstore.img`
5. Run the bash script `bash rpi-usb.sh`
6. Reboot the Pi `sudo reboot`



   
