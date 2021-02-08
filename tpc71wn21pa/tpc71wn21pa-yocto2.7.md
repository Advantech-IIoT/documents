# Yocto Project 2.7 (Warrior) for TPC71WN21PA

---

## Introduction

- Hardware: Advantech TPC71WN21PA
- CPU: i.MX6Q / RAM: DDR3 2GB
- Manifest base: imx-4.19.35-1.1.0.xml
- [Githib image builder source](https://github.com/advantechralph/yocto-local/tree/project/tpc71wn21pa-warrior)

---

## Build Image

### Prerequisite

- Host Setup
    - Recommand Ubuntu 18.04 or later. 
    - The minimum hard disk space required is about 50 GB. 
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
$ git clone https://github.com/advantechralph/yocto-local.git tpc71wn21pa-warrior
$ cd tpc71wn21pa-warrior
$ git checkout meta/tpc71wn21pa-warrior
$ git checkout project/tpc71wn21pa-warrior
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

###### tags: `tpc71wn21pa` `yocto`