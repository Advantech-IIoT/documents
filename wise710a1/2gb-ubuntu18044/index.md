# WISE710A1 DDR3 2GB Ubuntu18.04

## Contents
- [Quick Start](#quick-start)
- [Image Package](#image-package)
  - [Write Image to SD Card](#write-image-to-sD-card)
- [Switch SW2](#switch-sw2)
---

## Quick Start

### Boot from SD card boot loader

- SW2 switch on device, 1: OFF, 2: ON. (Boot from boot loader on SD card)

- Insert SD card into host Ubuntu 16.04 PC 

- Unpack image package, ex: wise710a1_2g_ubuntu18044_20200804.tar.gz

- Run script in the image package as below: 

  ```
  $ cd wise710a1_2g_ubuntu18044_20200804
  $ cd scripts
  $ ./mksd_recovery-linux_boot-from-sd.sh /dev/sdc ubuntu18044
  ```

- Insert SD card into WISE710A1

- Conenct RS232 cable between PC and WISE710A1. 

- Open console terminal and set baud rate to 115200 8n1. 

- Console login: root/123456

- Write SPL image into SPI flash. 

  On WISE710A1:
  
  ```
  $ cd /mk_inand/scripts
  $ ././mkspi-advboot.sh 
  ```
  
- Write image into eMMC

  ```
  $ ./mkinand-linux.sh /dev/mmcblk0 ubuntu18044
  ```

- Power down and change SW2 to boot from SPI flash. (SW2: 1: ON, 2: OFF)


## Image Package

### Package: wise710a1_2g_ubuntu18044_20200804

### Tree of this package: 

```
wise710a1_2g_ubuntu18044_20200804
├── image
└── scripts
```

### Write Image to SD Card

#### Host OS: Ubuntu 16.04 x86_64

Insert SD Card into host PC and check which block device. 

```
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sdb      8:16   0    2T  0 disk 
└─sdb1   8:17   0    2T  0 part /disk/workssd
sr0     11:0    1 1024M  0 rom  
sdc      8:32   1 14.4G  0 disk 
├─sdc2   8:34   1 14.4G  0 part 
└─sdc1   8:33   1   50M  0 part 
sda      8:0    0  512G  0 disk 
├─sda2   8:2    0    2G  0 part 
└─sda1   8:1    0  510G  0 part /
sr1     11:1    1 1024M  0 rom  
```

In this example case, the SD card block device is `sdc`. 

Then, enter the scripts sub-directory in this package. 

```
$ ls
image  scripts
$ cd scripts
```

Run the write image script with SD card block device.

```
# WISE710A1 boots from SPL on SPI flash and then load U-boot on SD card.
$ ./mksd_recovery-linux.sh /dev/sdc ubuntu18044
```

or 

```
# WISE710A1 boots from SD card boot loader directly. 
$ ./mksd_recovery-linux_boot-from-sd.sh /dev/sdc ubuntu18044
```

**Note:** Please refer to quick start for updating SPL. 

Follow the prompt to write image to SD card. 

```
All data on /dev/sdc now will be destroyed! Continue? [y/n]
```

Enter `y`, and wait for process done. 

#### Process prompt: 
```
partition start
DISK SIZE - 15502147584 bytes
partition done
partition done
mkfs.fat 3.0.28 (2015-05-16)
mkfs.fat: warning - lowercase labels might not work properly with DOS or Windows
mke2fs 1.42.13 (17-May-2015)
Creating filesystem with 3769344 4k blocks and 942848 inodes
Filesystem UUID: 28208437-5792-4f30-a6a1-1bed5d5edf7a
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: 

dd [adv_boot & u-boot]
copy [zImage & dtb]
copy [rootfs]
[Copying iNAND upgrate tools...]
chown: cannot access 'mount_point0/home/advantech': No such file or directory
umount: /dev/sdc: not mounted
umount: /dev/sdc1: not mounted
```

After process done, eject the SD card and insert it into WISE-710A1 2GB DDR3 device. 

## Switch SW2

The `SW2` switch on device, 1: ON, 2: OFF. (Boot from SPL on SPI flash)

The `SW2` switch on device, 1: OFF, 2: ON. (Boot from boot loader on SD card)

### Fig: boot from SPL on SPI flash

<img src="https://raw.githubusercontent.com/advantechralph/documents/master/wise710a1/2gb-ubuntu18044/001.jpg" alt="SW2" width="20%" height="20%">


