## Useful Guides

https://www.programmersought.com/article/88748078171/
https://hqmana.com/2021/07/20210727162839395e.html
https://www.programmersought.com/article/91255526107/


## Install Requirements
```sh
sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev 
sudo apt-get install libc6-dev lib32ncurses5-dev gcc-multilib x11proto-core-dev libx11-dev lib32z1-dev
sudo apt-get install  libgl1-mesa-dev g++-multilib tofrodos python-markdown libxml2-utils
sudo apt-get install pkg-config pkgconf zlib1g-dev libusb-1.0-0-dev
sudo apt-get install mingw32
sudo apt-get install gcc-arm-linux-gnueabihf
sudo apt-get install device-tree-compiler
sudo apt-get install mercurial
```


<hr>

## Default Password for Linux
```sh
with password “toortoor” 
pas  "licheepi"
```


## Compiler for ARCH

```sh
arm-xilinx-linux-gnueabi-gcc -v
export ARCH=arm
export CROSS_COMPILE=$(pwd)/gcc-linaro-5.5.0-2017.10-x86_64_arm-linux-gnueabi/bin/arm-linux-gnueabi-


export CROSS_COMPILE=$(pwd)/Xilinx_GNU_Linux/bin/arm-xilinx-linux-gnueabi-


export ARCH=arm
export CROSS_COMPILE=arm-xilinx-linux-gnueabi-
export PATH=/home/server/zturn_CD/Xilinx_GNU_Linux/bin:$PATH

export PATH=/home/server/zturn_CD/Xilinx_GNU_Linux/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

```

```sh
echo "export ARCH=arm" >> ~/.bashrc
echo "export CROSS_COMPILE=arm-xilinx-linux-gnueabi-" >> ~/.bashrc
echo "export PATH=/opt/cross_compiler/bin:$PATH" >> ~/.bashrc

```

## SHOW LINUX IMAGE
```sh
uname -srm  // show linux version
hostname   // show host name
cat /etc/os-release // show build root info
```

[!] After boot, terminal will reminder you input user name & password.
Please try root/toortoor，or root/licheepi

## Settings for Licheepi / Configs for make

```sh
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- LicheePi_Zero_800x480LCD_defconfig
```

<hr>

## Make LINUX

```sh
make ARCH=arm licheepi_zero_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j8
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j8 INSTALL_MOD_PATH=out modules
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j8 INSTALL_MOD_PATH=out modules_install
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- dtbs
```

## Convert the boot.cmd to boot.scr using mkimage

```sh
mkimage -C none -A arm -T script -d boot.cmd boot.scr
```

## Make linux modifiy make file

* Modify the top Makefile
* Modify the default compiler in Makefile364 in the kernel root directory.
* You can directly compile with make.

```sh
# ARCH		?= $(SUBARCH)
ARCH		?= arm
CROSS_COMPILE	?= arm-linux-gnueabihf-
sudo make licheepi_zero_defconfig
sudo make menuconfig   #add bluethooth, etc
sudo make -j8
make -j8 INSTALL_MOD_PATH=out modules
make -j16 INSTALL_MOD_PATH=out modules_install
make dtbs
```

## Generate files after compiling

Generate files after compiling

After compiling, zImage is under arch/arm/boot / and driver module is under out /.

The device tree file is in arch/arm/boot/dts /.

Device tree of common board: sun8i-v3s-licheepi-zero.dtb

Device tree of dock board: sun8i-v3s-licheepi-zero-dock.dtb

LCD_ Device tree of 480x272: sun8i-v3s-licheepi-zero-with-480x272-lcd.dtb

LCD_ Device tree of 800x480: sun8i-v3s-licheepi-zero-with-800x480-lcd.dtb

As a reminder, zero-4.10.y does not generate the next two dtb files
The latest zero-5.2.y files are generated.

<hr>

## Builderroot
```sh
# STEP 1
download   buildroot-2021.02.tar.gz    # Old Version
wget https://buildroot.org/downloads/buildroot-2019.08.tar.gz

# STEP 2
decompression
tar xvf buildroot-2019.08.tar.gz&&cd buildroot-2019.08/

# Configuration 
make menuconfig
```

**If the compilation fails**

```sh
sudo make clean
sudo make again
```
[!] _Compilation takes a long time, please wait._

### Configurations


>Target options –> Target Architecture 	ARM (little endian)

>Target options –> Target Architecture Variant 	cortex-A7

>Target options –> Target ABI 	EABIhf

>Target options –> Floating point strategy 	VFPv4

>System configuration –> System hostname 	Whatever you’d like!

>System configuration –> System banner 	Whatever you’d like!

>System configuration –> Enable root login with password 	Check if you’d like to secure the root login.

>System configuration –> Root password 	If you checked the above, this is the password to the root account.

>Target packages –> Interpreter languages and scripting –> micropython 	A simplified Python interpreter for embedded machines

>Target packages –> Shell and utilities –> file 	Returns information about a file

>Target packages –> Shell and utilities –> screen 	Allows for switching between multiple managed terminal jobs

>Target packages –> Shell and utilities –> ranger 	An improved file manager; requires additional toolchain support, though

>Target packages –> System tools –> htop 	An improved process viewer/manager

>Target packages –> Text editors and viewers –> nano 	A popular editor; requires wchar support

>Target packages –> Games –> * 	Install a few games for fun!

## How to make SD Card for boot and Run in LINUX

1) `ls /dev/sd*`

2) `sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdb bs=1024 seek=8`

## SD card editing

Create the image with `dd` Once we know the device name, we need the correct command to create the image of this device:

Open a terminal.
Type the following command:
```sh
sudo dd bs=4M if=/dev/sde of=/home/username/MyImage.img
```

Don’t forget to replace the device name (if for input file) and the file destination (of for output file).

mage restoration to the SD card
Copying back the image to another SD card is almost the same thing.
I recommend trying this at least one time, just to be sure that your image is working (don’t try on the same SD card!).

To copy an image to a new SD card, there are two ways you can use:

The first one is to use dd again, in the reverse order:
The command is something like:
```sh
sudo dd bs=4M if=/home/username/MyImage.img of=/dev/sde
```
For the first time, you need to edit this command with the correct path, image name and device name.

**Unmount if already mounted**

`$ sudo umount /dev/sdbx`

## Partition editing
`$ sudo fdisk /dev/sdb`
```
d　//Delete an existing partition
n　//Create 1st partition
p
1
+32M
n  //Create second partition
p
2
p
w
```

## Change Partition type
```sh
$ sudo mkfs.vfat /dev/sdb1
$ sudo mkfs.ext4 /dev/sdb2
```

## U-boot writin
```sh
$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdb bs=1024 seek=8
```

## Copy boot file
```sh
$ sudo mount /dev/sdb1 /mnt/boot
$ sudo cp zImage /mnt/boot/
$ sudo cp boot.scr /mnt/boot/
$ sudo cp suniv-f1c100s-licheepi-nano.dtb /mnt/boot
$ sync
$ sudo umount /dev/sdb1

# Copy of File System
$ sudo mount /dev/sdb2 /mnt/rootfs
$ sudo tar xvf rootfs.tar -C /mnt/rootfs/
```
<hr>

```sh
$ sudo mkdir -p /mnt/lib/modules
$ sudo rm -rf /mnt/lib/modules/
$ sudo cp -r <YOUR_MODULE_DIRECTORY>/output/lib /mnt/
```
<hr>

```sh
sudo cp ./rootfs.tar /mnt/
sudo tar -xf /mnt/rootfs.tar
sudo rm /mnt/rootfs.tar
sync
sudo umount /dev/sdX2
```

<hr>

## Zip all file to archive

```sh
sudo tar -zcvf rotfs_ok.tar.gz *
```

That was it - I had to go into the rootfs/bin directory and run 
sudo chown root * -R

problem solved.

```sh
chown /media/hadipic/rootfs/bin * -R
```
<hr>

```sh
tar vxf rootfs.tar
sudo chmod 777 /dev/ttyUSB0
./analogclock -platform linuxfb
```

<hr>

## Serial port setting for host connect

```sh

sudo adduser $USER dialout

sudo stty -F /dev/ttyUSB0 115200 -echo -icanon -onlcr parenb -parodd inpck ignpar

sudo minicom -D /dev/ttyUSB0

cat  /dev/ttyUSB0 speed 115200

# If custom password

openssl passwd -6 -salt xyz  qwert
openssl passwd -6 -salt xyz  yourpasssword

```

## Easy use minicom
Add it to Bash File
```sh
#!/bin/bash
sudo stty -F /dev/ttyUSB0 115200 -echo -icanon -onlcr parenb -parodd inpck ignpar
sleep 2
sudo minicom -D /dev/ttyUSB0
```

## reset pasword etc/shadow change

```sh
root::::::::
daemon:*:::::::
bin:*:::::::
sys:*:::::::
sync:*:::::::
mail:*:::::::
www-data:*:::::::
operator:*:::::::
nobody:*:::::::

## Custom Password for DEBIAN

root:$6$6IGoTLv2$40gg1mQGg1JlD4UKB33TFZqMjctQIGjH8kjSX7qUTGva.od3diJT4HjLBAfrzpWscdQWXxposo3J1laj86NUy0:17290:0:99999:7:::
daemon:*:17271:0:99999:7:::
bin:*:17271:0:99999:7:::
sys:*:17271:0:99999:7:::
sync:*:17271:0:99999:7:::
games:*:17271:0:99999:7:::
man:*:17271:0:99999:7:::
lp:*:17271:0:99999:7:::
mail:*:17271:0:99999:7:::
news:*:17271:0:99999:7:::
uucp:*:17271:0:99999:7:::
proxy:*:17271:0:99999:7:::
www-data:*:17271:0:99999:7:::
backup:*:17271:0:99999:7:::
list:*:17271:0:99999:7:::
irc:*:17271:0:99999:7:::
gnats:*:17271:0:99999:7:::
nobody:*:17271:0:99999:7:::
sshd:*:17271:0:99999:7:::
systemd-timesync:*:17271:0:99999:7:::
systemd-network:*:17271:0:99999:7:::
systemd-resolve:*:17271:0:99999:7:::
systemd-bus-proxy:*:17271:0:99999:7:::
messagebus:*:16842:0:99999:7:::
lightdm:*:16842:0:99999:7:::
dnsmasq:*:16842:0:99999:7:::

```

## Add console serial (Like Hyper-Terminal or Minicom)

`sudo nano /etc/inttab`

> Save this to file

```
console::respawn:/sbin/getty -L  console 0 vt100 # GENERIC_SERIAL
ttyS0::respawn:/sbin/getty -L ttyS0 115200 vt100 # tx rx hadi bashniji
```
<hr>

## qt sample test

Address : `/usr/lib/qt/examples/gui/analogclock/`

<hr>

## Audio Test

```sh
nano /etc/asound.conf
```
> Add Below to Config file

```conf
pcm.!default {
        type hw
        card 0
        device 0
}

ctl.!default {
        type hw
        card 0
}

pcm.!default{
        type hw
        card 0
        devive 0
}

ctl.!default{
        type hw
        card 0
        device 0
}
```

## Run test sound

[CODEC Document](http://zero.lichee.pro/%E9%A9%B1%E5%8A%A8/CODEC.html)

```sh
amixer -c 0 cset numid=12 2        record mic1
arecord -D hw:0,0 -d 3 -f S16_LE -r 16000 tmp.wav   录音测试

play

amixer -c 0 sset 'Headphone',0 100% unmute
speaker-test -twav -c2
atest
aplay  tmp.wav
```

to record sound.

The right status you can see:

```
root@Lichee:~/root# arecord -l

**** List of CAPTURE Hardware Devices ****

card 0: Codec [V3s Audio Codec], device 0: CDC PCM Codec-0 []

Subdevices: 1/1

Subdevice #0: subdevice #0

root@Lichee:~/root# ls /dev/snd/

by-path controlC0 pcmC0D0c pcmC0D0p timer

amixer -c 0 sset 'Headphone',0 100% unmute
```


## Save configuration with alsactl restore

You can store this sound configuration using the alsactl store command (as long as your filesystem is read/write capable).

After a reboot you can retrieve this configuration running alsactl restore.

<hr>

## How to mount boot
```sh
mount /dev/mmcblk0p1 /boot
```

<hr>

## Display platform setting add to bash rc to run QT Application

```sh
dmesg | grep fb0
export QT_QPA_PLATFORM=linuxfb:fb=/dev/fb0
export QT_QPA_EGLFS_TSLIB=1
export QT_QPA_GENERIC_PLUGINS=tslib,evdevkeyboard $QTDIR/examples/

./myap -platform "vnc:size=800x600:addr=127.0.0.1:mode=websocket"
 
export QT_QPA_PLATFORM=vnc

export QWS_SIZE=800x480
export QWS_DISPLAY=VNC:LinuxFb
export QWS_DEPTH=16
  
export QWS_DISPLAY="VNC:size=720x480:depth=32"
export QWS_DISPLAY="VNC:LinuxFb"
``` 
<hr>

 ## Display Settings :  fbset

```
mode "800x480-0"
        # D: 0.000 MHz, H: 0.000 kHz, V: 0.000 Hz
        geometry 800 480 800 480 32
        timings 0 0 0 0 0 0 0
        accel false
        rgba 8/16,8/8,8/0,0/0
endmode


lcd_x = 800
lcd_y = 480
lcd_dclk_freq = 33
lcd_hv_hspw = 23
lcd_hbp = 46
lcd_ht = 1055
lcd_hv_vspw = 1
lcd_vbp = 10
lcd_vt = 1050

lcd_x =             1024
lcd_y =             768
lcd_dclk_freq = 65
lcd_hv_vspw =       6
lcd_hv_hspw =       136
lcd_hbp =           180
lcd_ht =            1344
lcd_vbp =           29
lcd_vt =            1612

```

**تست کنید که آیا صفحه نمایش معمولی است یا خیر**


```sh
chvt 1
cat /dev/urandom > /dev/fb0
cat /dev/zero > /dev/fb0
```

<hr>

```sh
cat /proc/partitions
```

```
179        0    7815168 mmcblk0
 179        1      16384 mmcblk0p1
 179        2    4717568 mmcblk0p2
 179        3    3072000 mmcblk0p3
   8        0    7913472 sda
   8        1       2464 sda1
   
   8        2    6047744 sda2
```

<hr>

## led pin for Lichipie

```
SCD1CLK PG0 -G192 - Blue LED
SCD1CMD PG1 -G193 - Green LED
SCD1_D0 PG2 -G194 - Red LED
D1 PG3 -G195
D2 PG4 -G196
D3 PG5 -G197

SCD0_D1 -G160
SCD0_D0 -G161
SCD0CLK -(G162) -DoNotUse
SCD0CMD -(G163) -DoNotUse
SCD0_D3 -G164
SCD0_D2 -G165

U1TX -G149
U1RX -G150
```

Setup to control pin as output
Substitute 193 in the following commands for your GPIO number (GPIO, not pin number)

```sh
echo "193" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio193/direction
```

## Control Output State

```sh
echo "1" > /sys/class/gpio/gpio193/value
echo "0" > /sys/class/gpio/gpio193/value
```

<hr>


rc.local is a legacy from the System V init system. It is the last script to be executed before proceeding to a login screen for the desktop environment or a login prompt at the terminal. It's usually a Bash shell script, and you can run anything from the script.

Open or create /etc/rc.local file if it doesn't exist using your favourite editor as the root user.
`$ sudo vi /etc/rc.local`

Add placeholder code into the file.
```bash
#!/bin/bash

exit 0

Add command and logics to the file as necessary.
```

```bash
#!/bin/bash
/sbin/ip addr | grep inet\ | tail -n1 | awk '{ print $2 }' > /etc/issue
echo "" >> /etc/issue
exit 0
```

Set the file to executable.

`$ sudo chmod a+x /etc/rc.local`

<hr>

```
camera driver


                      csi1: camera@1cb4000 {
                        compatible = "allwinner,sun8i-v3s-csi";
                        reg = <0x01cb4000 0x3000>;
                        gic: interrupt-controller@1c81000 {
                        #interrupt-cells = <3>;
                        interrupts = <GIC_PPI 9 (GIC_CPU_MASK_SIMPLE(4) | IRQ_TYPE_LEVEL_HIGH)>;
                };
        
        
        
        
&i2c1 {
    	pinctrl-0 = <&i2c1_pins>;
		pinctrl-names = "default";
		clock-frequency = <400000>;
		status = "okay";
	
	ov2640: camera@30 {
		compatible = "ovti,ov2640";
		reg = <0x30>;
		pinctrl-names = "default";
		pinctrl-0 = <&csi1_mclk_pin>;
		clocks = <&ccu CLK_CSI1_MCLK>;
		clock-names = "xvclk";
		assigned-clocks = <&ccu CLK_CSI1_MCLK>;
		assigned-clock-rates = <24000000>;
		port {
			ov2640_0: endpoint {
				remote-endpoint = <&csi1_ep>;
				bus-width = <10>;
			};
		};
	};
};
```
<hr>

## lan in  linux z image

```
Device Drivers —>
 Network device support —>
  Ethernet driver support —>
   [*]   STMicroelectronics devices
     <*>     STMicroelectronics 10/100/1000/EQOS Ethernet driver
     <*>       STMMAC Platform bus support
     < >         Support for snps,dwc-qos-ethernet.txt DT binding.
     <*>         Generic driver for DWMAC
     <*>         Allwinner GMAC support
     <*>         Allwinner sun8i GMAC suppor
```     

<hr>

## dvb-t

```sh
gnutv -channels ~/channels.conf -out file somefilename STATION_NAME

w_scan -fc -c IR -M   -C UTF-8> channels.conf

w_scan -ft -c IR -L > dvb.xspf
vlc dvb.xspf 

vlc dvb://frequency=543000000 :program=3
``` 

<hr>

## how to wifi seting and instal

> make usb dongel

```sh
git clone --recursive

git clone -b arm https://github.com/kelebek333/rtl8188fu rtl8188fu-arm

make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- KSRC=~/liceepi/linux

cp rtl8188fu.ko   /lib/modules/net/wireless/
or
cp r8723bs.ko   /lib/modules/net/wireless/

# make script and copy comand

/etc/wpa_supplicant.conf

ap_scan=1
network={
        ssid="shop-electronic.ir"
        psk="bashniji"
        key_mgmt=WPA-PSK
      }

```

<hr>


`nano /root/wifi.sh`

```sh
#!/bin/bash
sleep 5
echo "Run cfg80211.ko"
insmod /lib/modules/net/wireless/cfg80211.ko
echo "Inserting WiFi Module "
insmod /lib/modules/net/wireless/rtl8188fu.ko
echo "WiFi Module inserted! rtl8188 "
sleep 5
/sbin/ifconfig wlan0 up
echo  "wlan start"
```

`sudo chmod 777 /root/wifi.sh`

**add tostart up linux**
 
```
root@LicheePi:~# nano /etc/rc.local
Add:
sudo -u root /root/wifi.sh 
``` 

<hr>

```sh
 cd /root برای افزودن اسکریپت connect_wx
 nano connect_wx.sh
 
 #!/bin/sh
insmod /usr/lib/r8723bs.ko
ifconfig wlan0 up
wpa_passphrase ssid pasword | tee /etc/wpasupplicant.conf

wpa_supplicant -B -d -i wlan0 -c /etc/wpa_supplicant.conf
udhcpc -i wlan0


#!/bin/sh
wpa_supplicant -Dnl80211 -iwlan0 -c/etc/wpa_supplicant.conf &
udhcpc -i wlan0

chmod 777 connect_wx.sh

nano /etc/init.d/Startup
/wifi.sh
```

<hr>

scan wifi

```sh
sudo iwlist wlan0 scan | grep ESSID

iwconfig wlan0 essid 'ali' key s:'bashniji'

ifup -a

ip addr show wlan0

nano /etc/network/interfaces
```

# interface file auto-generated by buildroot

```sh
auto lo

iface lo inet loopback

iface wlan0 inet manual

iface wlan0 inet dhcp

auto wlan0

       wpa-ssid {ali}
       wpa-psk  {bashniji}
```

## QT

```sh
 netstat -a | grep 5900
```

## Run Application in another ARCH Device

So once x11vnc is installed, you can use it like this:

1) Start your Qt5 application:

``./myCuteQt551App -platform linuxfb -geometry WxH &``

Where W and H are respectively the Width and the Height of your graphical view (in integer, for instance 240x320).

2) Start your x11vnc server

``x11vnc -noipv6 -rawfb /dev/fb0 -clip WxH+0+0``


## Compile QT for ARCH in Windows
```sh
configure.bat  -xplatform linux-arm-gnueabi-g++ -v -developer-build -opensource -prefix  C:\Qt\Qt5.14.2\5.14.2\arm  -release -qml -sql-sqlite  -dbus -no-opengl


./configure -opensource -confirm-license -xplatform win32-g++ -device-option CROSS_COMPILE=/usr/bin/x86_64-w64-mingw32 -prefix /usr/Qt/5.13.2/x86_64-w64-mingw32 -nomake examples
```

<hr>

```sh
# This is a minimal sample config file, which can be copied to
# /etc/X11/xorg.conf in order to make the Xorg server pick up
# and load xf86-video-fbturbo driver installed in the system.
#
# When troubleshooting, check /var/log/Xorg.0.log for the debugging
# output and error messages.


Section "Device"
        Identifier "Sfbo0"
        Driver "fbdev"
        Option "fbdev" "/dev/fb0"
   #      Option         "Rotate"    "CW"
         Option          "SwapbuffersWait" "true"
  
EndSection

Section "Monitor"
        Identifier   "Monitor0"
        VendorName   "Monitor Vendor"
        ModelName    "Monitor Model"
EndSection

Section "Screen"
        Identifier "My Screen"
        Device "Sfbo0"
        Monitor "Monitor"        
EndSection

Section "Files"
        ModulePath   "/usr/lib/xorg/modules"
         FontPath     "/etc/X11/fonts/75dpi/"
EndSection

Section "InputDevice"
        Identifier  "Keyboard0"
        Driver      "kbd"
EndSection

Section "InputDevice"
        Identifier  "Mouse0"
        Driver      "mouse"
        Option      "Protocol" "auto"
        Option      "Device" "/dev/input/mouse1"
#        Option      "ZAxisMapping" "4 5 6 7"
EndSection

Section "ServerLayout"
        Identifier     "X.org Configured"
        Screen      0  "My Screen" 0 0
        InputDevice    "Mouse0" "CorePointer"
        InputDevice    "Keyboard0" "CoreKeyboard"
EndSection
```

<hr>

## Make bash file for init settings
## Run Application to ARCH

```sh
export DISPLAY=:0
amixer -c 0 sset 'Headphone',0 100% unmute
QT_QPA_PLATFORM=linuxfb:fb=/dev/fb0
```
