# Yocto Project 2.7 (Warrior) for TPC71WN21PA

---

## Introduction

- Hardware: Advantech TPC71WN21PA
- CPU: i.MX6Q / RAM: DDR3 2GB
- Manifest base: imx-4.19.35-1.1.0.xml

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

## References

- [IMX_YOCTO_PROJECT_USERS_GUIDE](https://www.nxp.com/docs/en/user-guide/IMX_YOCTO_PROJECT_USERS_GUIDE.pdf)
- [imx-manifest-warrior](https://source.codeaurora.org/external/imx/imx-manifest/refs/?h=imx-linux-warrior)
- [Yocto Project Reference Manual](https://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html)

---

###### tags: `tpc71wn21pa` `yocto`