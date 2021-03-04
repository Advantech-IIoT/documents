
# Yocto 2.7 for WISE710A1

---

## Prerequisites

### Host Machine

MacOS and Windows 10 install VirtualBox virtual machine and run Ubuntu 16.04 64-bit OS. Or run native Ubuntu 16.04 on x86 64-bit.

### Host Setup

#### Build environment ([Yocto project supported distributions](https://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#detailed-supported-distros)):
- Host: Ubuntu 16.04 x86_64
- Packages for Ubuntu
  - **Essentials:** Packages needed to build an image on a headless system:
    ```shell
    $ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
      build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
      xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
      pylint3 xterm
    ```
  - **Documentation:** Packages needed if you are going to build out the Yocto Project documentation manuals:
    ```shell
    $ sudo apt-get install make xsltproc docbook-utils fop dblatex xmlto
    ```
  - **Kernel menuconfig**
    ```shell
    $ sudo apt-get install libncurses5-dev
    ```

---

## Quick Start 

The quick start example is based on Ubuntu 16.04 x86_64 on VirtualBox virtual machine.
It includes the packages needed to install on Ubuntu 16.04 and fetch build package on
Github. After done for prerequisites, make image by makefile script.

### Host setup

```
  $ sudo apt-get update -y && apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib \
  build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
  xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
  pylint3 xterm
  $ sudo apt-get install -y make xsltproc docbook-utils fop dblatex xmlto vim 
```

### Fetch source from github. 

#### Git configuration

```
$ git config --global user.name "<Your-Name>"
$ git config --global user.email "<Your-Mail-Address>"
$ git config --global core.editor vim
```

#### Clone source

```
$ git clone -b project/wise710a1-warrior https://github.com/advantechralph/yocto.git
```

### Set up Yocto's working directory (sstate-cache, tmp, and downloads).  

```
$ cd yocto
$ YOCTO_WORK_DIR=/root/imx-linux-warrior make prepare
```

### Build NXP (Freescale) Yocto full command line image (core-image-full-cmdline)

```
$ make buildcmdline
```

## Write image to SD Card

After you build core-image-full-cmdline image, you can run `make writesd` and follow the prompt
to write image to write image to SD card.

### Detial information: 

The below information will show how `make writesd` works for writing image to SD card.
You can also follow the steps as below.

1. Check the image's path.

```
$ make info | grep core-image-full-cmdline
core-image-full-cmdline       : /disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard
```

2. Prepare micro SD card. 

Prepare a micro SD card and insert to host Ubuntu 16.04 machine. You can type command as below to check disk device. 

ex: (16GB micro SD Card )
```
$ lsblk -d
NAME MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sdb    8:16   0    2T  0 disk 
sr0   11:0    1 1024M  0 rom  
sdc    8:32   1 14.4G  0 disk 
sda    8:0    0  512G  0 disk 
sr1   11:1    1 1024M  0 rom  
```

3. Write image by 'dd' command. 

After you get the image's path and SD card device name, you can use `dd` command to write image to SD card. 

```
# ex: 
# ImagePath=/disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard
# SD_dev=/dev/sdc
$ dd if=${ImagePath} of=${SD_dev} bs=1M status=progress conv=fsync
$ sync
```

4. All commands you needs.  

```
$ make info | grep core-image-full-cmdline
$ lsblk -d
$ dd if=/disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard of=/dev/sdc bs=1M status=progress conv=fsync
$ sync
```

---

## Write image to eMMC on device

**Preparation:** Check if the WISE-710A1 gets IP address by DHCP and you can PING to it. The script will write image to eMMC by ssh connection. 

After you build core-image-full-cmdline image, you can run `make emmcqt5` and follow the prompt 
to write image to eMMC.

**Important:** Make sure device boot from SD card image and then run the command previous mentioned. 

### Detial information: 

The below information will show how `make emmcqt5` works for writing image to eMMC. 
You can also follow the steps as below. 

1. Check the image's path.  

```
$ make info | grep core-image-full-cmdline
core-image-full-cmdline       : /disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard
```

2. Chec device boot from SD card. 

Check the device boot from SD card and mount root filesystem on the partition of SD card. 

```
$ mount | grep " / "
```

If it shows `/dev/mmcblk1pX`, the 'X' is a partition number and the 'mmcblk1' means at SD card. 

3. Connect the device to the intranet. 

Connect the WISE-710A1 to the intranet with DHCP server for simple usage, and get the IP address of the device. 

```
# ex: 
# WISE710A1_IP= 172.17.8.183
$ ping ${WISE710A1_IP}
```

4. Unmount file system for any eMMC partition through SSH. 

By default, Yocto will mount media into '/run/media' and eMMC partitions will be '/run/media/mmcblk0pX', 
the 'X' is a partition number. 

```
# ex: 
# WISE710A1_IP= 172.17.8.183
# Remove all keys in known_hosts 
$ ssh-keygen -R ${WISE710A1_IP}
# Use ssh to send unmount commands on WISE710A1
$ ssh root@${WISE710A1_IP} umount /run/media/mmcblk0p?
```

5. Write image to eMMC by scp command. 

```
# ex: 
# ImagePath=/disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard
# WISE710A1_IP= 172.17.8.183
# Use scp to send image to eMMC 
$ scp ${ImagePath} root@${WISE710A1_IP}:/dev/mmcblk0
$ ssh root@${WISE710A1_IP} sync
```

6. All commands you need

```
# ex: 
# ImagePath=/disk/workssd/advantechralph/workdir/warrior/tmp/deploy/images/imx6dlwise710a1/core-image-full-cmdline-imx6dlwise710a1-20200611073255.rootfs.sdcard
# WISE710A1_IP= 172.17.8.183
$ ping ${WISE710A1_IP}
$ scp ${ImagePath} root@${WISE710A1_IP}:/dev/mmcblk0
$ ssh root@${WISE710A1_IP} sync
```

---

## Build Package 

- [Package Gitub Link](https://github.com/advantechralph/yocto/tree/project/wise710a1-warrior)

- Package include: 

  - Makefiles
    - Set up the Yocto directories(sstate-cache, tmp, and downloads). 
    - Rules to fetch repo and synchronize the Yocto meta layers source by manifest. 
    - Rules to create the Yocto images, ex: core-image-minimal, , core-image-full-cmdline, fsl-image-qt5 and etc. 

  - BSP scripts
    - Script to write boot loader, kernel image and root file system to SD card.
    - Script to write images to eMMC. 

- Build package file structure:

  ```
    ├── build                              # Yocto project building folder
    ├── default.xml                        # xml manifest
    ├── fsl-setup-release.sh
    ├── include                            # included makefiles
    ├── macro.mk                           # global parameters for makefile
    ├── Makefile                           # Makefile for make target
    ├── README-IMXBSP
    ├── setup-environment
    └── yocto.build.mk

  ```

- Package file structure after run 'make prepare':

  ```
    ├── build
    │   ├── bin                            # repo tool
    │   │   ├── docs
    │   │   ├── hooks
    │   │   ├── subcmds
    │   │   └── tests
    │   │       └── fixtures
    │   └── yocto                          # Yocto project folder
    │       ├── build.imx6dlwise710a1    # Yocto build folder
    │       │   ├── cache
    │       │   ├── conf
    │       │   └── workdir
    │       ├── sources                    # meta layers folder
    │       │   ├── base
    │       │   ├── meta-advantech
    │       │   ├── meta-browser
    │       │   ├── meta-fsl-arm
    │       │   ├── meta-fsl-arm-extra
    │       │   ├── meta-fsl-bsp-release
    │       │   ├── meta-fsl-demos
    │       │   ├── meta-openembedded
    │       │   ├── meta-qt5
    │       │   ├── meta-wise710a1
    │       │   └── poky
    │       └── wise710a1               # build package
    │           └── include
    ├── default.xml
    ├── fsl-setup-release.sh
    ├── include
    ├── macro.mk
    ├── Makefile
    ├── README-IMXBSP
    ├── setup-environment
    └── yocto.build.mk

  ```

---

## Yocto Tools

### Host, source and Yocto's working directory
Done for "Host setup", "Fetch source from github", and "Configure working directory" sections in Quick Start. 

### Set up environment variables

```
$ cd build/yocto
$ source setup-environment build.imx6dlwise710a1
```

### Bitbake usages
   
#### list versions

```
$ bitbake -s
```

#### Build images

```
$ bitbake fsl-image-qt5 
# or 
$ bitbake core-image-minimal
# or
$ bitbake core-image-full-cmdline
```

### devtool

#### Modify package source and create patches. 

1. Create current source into workspace

```
$ devtool modify u-boot-imx
$ cd workspace/sources/u-boot-imx

# Done some modification...
       
$ cd -                         # back to build.imx6dlwise710a1
$ bitbake u-boot-imx           # test build for modification

```

2. After modify source in `u-boot-imx`, run git add and commit for branch `devtool`. 

```
$ git add .
$ git commit . -m "add some comment here..." 
```

3. Update recipe to layer foler. 
     
```
$ bitbake-layers create-layer --priority 15 meta-test
$ devtool update-recipe -a meta-test u-boot-imx      # all patches will be located in meta-test
```

---

###### tags: `wise710a1`, `yocto`, `nxp`, `i.mx6`