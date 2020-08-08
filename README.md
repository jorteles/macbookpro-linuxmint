# Macbook Pro - Linuxmint 19.3
- My Linuxmint 19.3 configuration to Macbook Pro 2009 intel i5 2.3Ghz

# Check switcheroo

sudo grep -i switcheroo /boot/config-*

- The vga_switcheroo mechanism will only be active when the kernel is booted with either the "modeset=1" kernel option, and/or the "nomodeset" option being absent.

sudo gedit /etc/default/grub

- In that text file there should be a:

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nvidia.modeset=1"

update-grub

Reference: https://help.ubuntu.com/community/HybridGraphics

sudo vim /etc/grub.d/01_enable_vga.conf

cat << EOF
setpci -s "00:01.0" 3e.b=8
setpci -s "01:00.0" 04.b=7
EOF

chmod 755 /etc/grub.d/01_enable_vga.conf

- Install nvidia driver by driver manager Linuxmint|Ubuntu
- Don't change any configuration in Nvidia-settings. It might increase your CPU usage by cynnamon. I don't know why by the way.

# Change fn key behavior - F1 to F12

- Enable before apply to hid_apple.conf file

echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode
echo 2 | sudo tee /sys/module/hid_apple/parameters/swap_opt_cmd

- Put in the file hid_apple.conf in /etc/modprobe.d

echo options hid_apple fnmode=2 | sudo tee /etc/modprobe.d/hid_apple.conf 
echo options hid_apple swap_opt_cmd=2 | sudo tee >> /etc/modprobe.d/hid_apple.conf

# Swaping ejectcd to delete key and rigth fn to right ctrl

- clone repository https://github.com/free5lot/hid-apple-patched and follow
  - In the repository folder type make and wait for compiling.
  - Rename /lib/modules/5.4.55-linux-image-06082020/kernel/drivers/hid/hid_apple

sudo mv /lib/modules/5.4.55-linux-image-06082020/kernel/drivers/hid/hid_apple.ko /lib/modules/5.4.55-linux-image-06082020/kernel/drivers/hid/hid_apple.ko.old
cp hid_apple.ko /lib/modules/5.4.55-linux-image-06082020/kernel/drivers/hid/

  - Enable before apply to hid_apple.conf file

echo 1 | sudo tee /sys/module/hid_apple/parameters/swap_fn_leftctrl
echo 1 | sudo tee /sys/module/hid_apple/parameters/ejectcd_as_delete

  - Add to your /etc/modprobe.d/hid_apple.conf

options hid_apple swap_fn_leftctrl=1
options hid_apple ejectcd_as_delete=1

  - Applying changes after test

echo options hid_apple swap_fn_leftctrl=1 | sudo tee /etc/modprobe.d/hid_apple.conf 
echo options hid_apple ejectcd_as_delete=1 | sudo tee >> /etc/modprobe.d/hid_apple.conf

sudo update-initramfs -u

- References: https://help.ubuntu.com/community/AppleKeyboard, https://github.com/free5lot/hid-apple-patched and follow

# Cedilha
- Mapping keys toproduce cedilha with ' and c.

GTK_IM_MODULE=cedilla
QT_IM_MODULE=cedilla

- Reference: http://wiki.linuxquestions.org/wiki/List_of_Keysyms_Recognised_by_Xmodmap

- High CPU consuption by cynnamon: Disable cynnamon software 3D
echo CINNAMON_2D=true
