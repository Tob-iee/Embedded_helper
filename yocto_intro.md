# BUILDING YOCTO PROJECT FOR RASPBERRY PI

## Intro
Who Is This Tutorial For?

This tutorial is NOT for folks that just want to get Linux up and running with a nice GUI so they can use a video game emulator or host a media server. For that, Raspbian or NOOBs should do the job, and it will be far easier to get things up and running that way than what I'm going to be describing.

The intended audience for this tutorial are people that either:
Are just starting to use Yocto and want some examples to follow on a cheap, widely available development board,
Need to create a custom image with included packages that they'll be distributing on a large number of boards, or
Want to figure out how to generate a customized device tree, ramdisk root filesystem, or bootloader.
Want to gain a better understanding of what basic files are needed to get Linux up and running on a Raspberry Pi.
Or maybe you just want to see what it would take to get Linux up and running with Yocto before you commit to using it for your embedded solution. Who knows? It's possible you found this page for some really cool reason I'm not even aware of.

https://cdn.sparkfun.com/assets/2/5/c/4/5/50e1ce8bce395fb62b000000.png

sudo screen /dev/tty.usbserial-AO008799 115200

## Package pre-requisites for Yocto project
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm python

## Downloading Yocto: Zeus Branch
$ cd ~
$ mkdir Yocto
$ cd Yocto
$ git clone -b zeus http://git.yoctoproject.org/git/poky

## Source bitbake build script
$ source poky/oe-init-build-env build

## Download meta-raspberry pi layer
$ cd ~/Yocto or cd ..
$ git clone -b zeus git://git.yoctoproject.org/meta-raspberrypi

## Modify build configuration files (i.e.bblayers.conf and local.conf)
bblayers tells bitbake where various layers are found
local is used to configure the target of the build and output location of the build

### bblayers.conf
$ vim ~/Yocto/build/conf/bblayers.conf

```
# POKY_BBLAYERS_CONF_VERSION is increased each time build/conf/bblayers.conf 
# changes incompatibly 
POKY_BBLAYERS_CONF_VERSION = "2" 

BBPATH = "${TOPDIR}" 
BBFILES ?= "" 

BBLAYERS ?= " \ 
/home/${USER}/Yocto/poky/meta \ 
/home/${USER}/Yocto/poky/meta-poky \ 
/home/${USER}/Yocto/poky/meta-yocto-bsp \ 
/home/${USER}/Yocto/meta-raspberrypi \ 
/home/${USER}/Yocto/meta-rpilinux \
" 
```

### local.conf
$ vim ~/Yocto/build/conf/local.conf
Go to the end of the file and append 
```
MACHINE ?= "raspberrypi3-64"
PREFERRED_VERSION_python3 = "3.6%"
PREFERRED_VERSION_python3-native = "3.6%"

ENABLE_UART = "1"
INIT_MANAGER = "systemd"
RPI_USE_U_BOOT = "1"
```

To see the content of raspberrypi4-64.conf go to
$ cat ~/Yocto/meta-raspberrypi/conf/machine/raspberrypi3-64.conf


## Create our custom layer
$ cd ~/Yocto 
$ mkdir meta-rpilinux 
$ cd meta-rpilinux 
$ mkdir conf 
$ cd conf && vi layer.conf

```
# We have a conf and classes directory, add to BBPATH
BBPATH .= ":${LAYERDIR}"

# We have recipes-* directories, add to BBFILES
BBFILES += "${LAYERDIR}/recipes-*/*/*.bb \
${LAYERDIR}/recipes-*/*/*.bbappend"

BBFILE_COLLECTIONS += "rpilinux"
BBFILE_PATTERN_rpilinux = "^${LAYERDIR}/"
BBFILE_PRIORITY_rpilinux = "6"

LAYERSERIES_COMPAT_rpilinux = "zeus dunfell"
```

## Create our embedded recipe
$ cd ~/Yocto/meta-rpilinux 
$ mkdir recipes-rpilinux 
$ cd recipes-rpilinux 
$ mkdir images 
$ cd images 
$ vi rpilinux-image.bb

Add the following to rpi-linux-image.bb
```
require recipes-core/images/core-image-minimal.bb 

IMAGE_INSTALL += "libstdc++ mtd-utils" 
IMAGE_INSTALL += "openssh openssl openssh-sftp-server"
```

## PARTY TIME 
$ cd ~/Yocto/build
$ bitbake rpilinux-image

# looks at this site for image review
https://gachiemchiep.github.io/cheatsheet/build_image_rpi3_yocto/
https://kickstartembedded.com/2021/12/22/yocto-part-4-building-a-basic-image-for-raspberry-pi/
https://kickstartembedded.com/2022/01/21/yocto-part-6-understanding-and-creating-your-first-custom-recipe/
https://www.yoctoproject.org/docs/1.8/bsp-guide/bsp-guide.html
