# Yocto Project 2.7 (Warrior) for TPC71WN21PA

---

## Introduction

- Hardware: Advantech TPC71WN21PA
- CPU: i.MX6Q / RAM: DDR3 2GB
- Manifest base: imx-4.19.35-1.1.0.xml
- [Yocto image builder source on Github](https://github.com/Advantech-IIoT/Yocto2.7-TPC-71W-N21PA/tree/project/tpc71wn21pa-warrior)

---

## Build Image

### Prerequisite

- Host Setup
    - Recommand Ubuntu 18.04 or later. 
    - It is recommended that at least 120 GB is provided. 
    - Install the essential Yocto Project host packages. 
    - For more detail information , refer to the chapter 3 in [IMX_YOCTO_PROJECT_USERS_GUIDE](https://www.nxp.com/docs/en/user-guide/IMX_YOCTO_PROJECT_USERS_GUIDE.pdf). 

```bash=
$ sudo apt-get install gawk wget git-core diffstat \
    unzip texinfo gcc-multilib build-essential chrpath \
    socat cpio python python3 python3-pip python3-pexpect \
    xz-utils debianutils iputils-ping python3-git \
    python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm
```

### Step 1. Clone image builder from Github

```bash=
$ git clone git@github.com:Advantech-IIoT/Yocto2.7-TPC-71W-N21PA.git
```

### Step 2. Prepare Yocto building enviroment 

```bash=
$ make yocto
```

### Step 3. Build QT5 image

```bash=
$ make imageqt5
```

---

## Write Image into SD card

Insert the SD card into the build code Ubuntu Host. 

```bash=
$ make writesdcard
```
Then, follow the prompt to write the image into sdcard. 

---

## Build toolchain

```bash=
$ make toolchain
```

For the toolchian self-installation script, please refer to the sdk in `Path information` as below. 

---

## Path information

```bash=
$ make info

# fsl-image-qt5   : qt5 image
# ...
# sdk             : sdk file
# ...
```

---

## Install SDK toolchain

After use `make info`, you will get the sdk location. You can execute this script to install toolchain on another Ubuntu host environment. 

```bash=
$ apt-get install xz-utils build-essential python python3 
$ ~/Download/fsl-imx-x11-glibc-x86_64-meta-toolchain-qt5-cortexa9hf-neon-toolchain-4.19-warrior.sh
```

Follow the prompt, we install toolchian in the default location `/opt/fsl-imx-x11/4.19-warrior`. 
For using toolchian, use the command as below to setup compiling environment. 

```bash=
$ . /opt/fsl-imx-x11/4.19-warrior/environment-setup-cortexa9hf-neon-poky-linux-gnueabi
```

Check the `$CC` variable. 

```bash=
$ echo $CC
arm-poky-linux-gnueabi-gcc -mfpu=neon -mfloat-abi=hard -mcpu=cortex-a9 --sysroot=/opt/fsl-imx-x11/4.19-warrior/sysroots/cortexa9hf-neon-poky-linux-gnueabi
```

Write a `hello.c` and test to compile it. 

```c=
#include <stdio.h>
int main(int argc, char *argv[]){
  printf("%s, %d:  hello\n", __FUNCTION__, __LINE__);
  return 0; 
}
```

```bash=
$ $CC -Wall -o hello hello.c
```

Then, upload `hello` to your tpc71wn21pa and execute it to test, ex: `scp`. 

```bash=
root@imx6qtpc71wn21pa:~# ./test 
main, 3: hello
```

---

<!--
## Useful information

### Offical NXP i.MX6 Yocto project source

For official NXP i.MX6DualLite/i.MX6Q Yocto 2.7 project, please fetch manifest as below. 

```bash=
$ repo init -u https://source.codeaurora.org/external/imx/imx-manifest \
  -b imx-linux-warrior -m imx-4.19.35-1.1.0.xml
$ repo sync  
```

```bash=
$ DISTRO=<distro name> MACHINE=<machine name> source imx-setup-release.sh -b <build dir>
$ cd <build dir>
```

---

-->

## References

- [IMX_YOCTO_PROJECT_USERS_GUIDE](https://www.nxp.com/docs/en/user-guide/IMX_YOCTO_PROJECT_USERS_GUIDE.pdf)
- [imx-manifest-warrior](https://source.codeaurora.org/external/imx/imx-manifest/refs/?h=imx-linux-warrior)
- [Yocto Project Reference Manual](https://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html)

---

## Advanced

----

### Rules and process control makefiles

```bash=
$ ls include/
bsp.mk  builddir.mk  build.mk  clean.mk  color.mk  gitserver.mk  image.mk  info.mk  prepare.mk  repo.mk  sdcard.mk  toolchain.mk  usage.mk  yocto.mk
```

- Yocto environment: `include/yocto.mk`
- Yocto build command: `include/build.mk`
- Build image: `include/image.mk`
- Image upgrade package: `include/bsp.mk`

---

### Use Yocto project tools - bitbake, devtool...

After done for "`make yocto`" or "`make imageqt5`", follow the commands as below to setup yocto env. 

```bash=
$ cd build/yocto
$ source setup-environment build.imx6qtpc71wn21pa/
ERROR: do not use the BSP as root. Exiting...

Welcome to Freescale Community BSP

The Yocto Project has extensive documentation about OE including a
reference manual which can be found at:
    http://yoctoproject.org/documentation

(...)

$ bitbake --help

...

$ devtool --help

...

```

---

### Modify package source workflow

#### Manual setup yocto env

From source package root directory. 

```bash=
$ cd build/yocto
$ source setup-environment build.imx6qtpc71wn21pa
```

Then, you will be in `build.imx6qtpc71wn21pa`.     

If you want to edit kernel source,    
use `devtool` and commands as below to put source code into workspace. 

```bash=
$ devtool modify linux-imx
```

After fetch code done, enter the kernel source workspace. 

```bash=
$ cd workspace/sources/linux-imx
```

Check git hash with commands as below. 

```bash=
git log --oneline --decorate 
80bfc285af33 (HEAD -> devtool, tag: list, tag: devtool-patched) fix mcu reboot unstable
fde0496926dc enable pci controller
fe8cf7e978cf io reset
00c32ddd4862 mcu reboot
abdd4541e9df fine tune realtek phy
672ecff90190 clock for rgmii
aa3b53191219 update for mcu watchdog
2cafae0f6a57 config and dts for tpc71wn21pa
8507afc3a397 (tag: devtool-base, origin/imx_4.19.35_1.1.0, imx_4.19.35_1.1.0) MLK-23846 ARM64: dts: freescale: fsl-imx8mn-ddr4-evk: correct bd71847 device node

```

- devtool-base: official release
- devtool-patched: currently patched
- devtool-base: workspace commit


Modify the kernel source and re-build kernel image with `bitbake linux-imx` command. 

Commit the source code by `git commit` command. 

After you commit the code, the `devtool` branch will move forward. 

```bash=
git log --branches="*devtool*" --oneline --decorate 
84e43b1bc090 (HEAD -> devtool) add dummy file
80bfc285af33 (tag: list, tag: devtool-patched) fix mcu reboot unstable
fde0496926dc enable pci controller
fe8cf7e978cf io reset
00c32ddd4862 mcu reboot
abdd4541e9df fine tune realtek phy
672ecff90190 clock for rgmii
aa3b53191219 update for mcu watchdog
2cafae0f6a57 config and dts for tpc71wn21pa
8507afc3a397 (tag: devtool-base, origin/imx_4.19.35_1.1.0, imx_4.19.35_1.1.0) MLK-23846 ARM64: dts: freescale: fsl-imx8mn-ddr4-evk: correct bd71847 device node

```

The `devtool` branch  will be ahead of `devbtool-patched`. 
Then, you can use `devtool update-recipe` command to create patches. 

Before updating, it is recommended to create a working layer instead to create patches directly to the original meta/recipe. 


Create a dummy meta layer. 

```bash=
$ bitbake-layers create-layer -p 15 meta-dummy 
```

Create patches into this dummy meta. 

```bash=
$ devtool update-recipe -a meta-dummy linux-imx 
```

Check the files structure of the dummy layer. 

```bash=
tree meta-dummy/
meta-dummy/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
├── recipes-example
│   └── example
│       └── example_0.1.bb
└── recipes-kernel
    └── linux
        ├── linux-imx
        │   └── 0001-add-dummy-file.patch
        └── linux-imx_4.19.35.bbappend

```

The patch `0001-add-dummy-file.patch` is created, and you can add this patch into original location of kernel recipe. 

```bash=
$ mv 0001-add-dummy-file.patch ${yocto pacakge rootdir}/build/yocto/sources/meta-advantech/meta-bsp-patch/recipes-kernel/linux/linux-imx
```

Append patch file into `SRC_URI` this variable in the `linux-imx_4.19.35.bbappend`. 


${yocto pacakge rootdir}/build/yocto/sources/meta-advantech/meta-bsp-patch/recipes-kernel/linux/linux-imx_4.19.35.bbappend: 

```bash=
ILESEXTRAPATHS_prepend := "${THISDIR}/${PN}:"

SRC_URI += "file://0001-config-and-dts.patch \
            file://0002-mcu-watchdog.patch \
            file://0003-fec-clock.patch \
            file://0004-rtl8211f.patch \
            file://0005-mcu-reboot.patch \
            file://0006-io-reset.patch \
            file://0001-enable-pci-controller.patch \
            file://0001-fix-mcu-reboot-unstable.patch \
            file://0001-add-dummy-file.patch \
            "
LOCALVERSION = ""
```

After update recipe and patches, do not forget to make package leave workspace. 

```bash=
$ devtool reset linux-imx
```


---

###### tags: `tpc71wn21pa` `yocto`
