# WISE710A1 Ubuntu18.04 BSP Builder

---

## Contents

- [Quick Start](#quick-start) 
- [WISE710A1 Boot From SD/eMMC](#wise710a1-boot-from-sdemmc) 

---

## Quick Start

### Clone Source

Clone the bsp builder from Github repository. 

```bash=
$ git clone https://github.com/Advantech-IIoT/Ubuntu18.04-WISE-710A1.git
$ cd wise710a1
```

### Make BSP

Execute make command to build BSP. All the result will be located in the 'build' folder. 

```bash=
$ make bsp modelname=wise710a1
$ ls build
```

#### For 2GB model

```bash=
$ make bsp modelname=wise710a1_2G
$ ls build
```

### Write Image to SD Card

Prepare a micro SD card and Linux host (ex: Ubuntu 16.04). Insert it into Linux host with the card reader. 
You can use the commands as below to check SD card devices. 

```
$ lsblk -d
or 
$ fdisk -l
```

After you confirm your SD card device (ex: /dev/sdc), enter the 'scripts' folder and run the
write image to SD card script.  

```
$ cd build/scripts
$ ./mkmksd_recovery-linux.sh /dev/sdc ubuntu18044
```

Follow the prompt to write image to SD card.Wait for success prompt and remove the micro SD card from reader. 
Then, put the SD card to the WISE710A1 and power on to test. 


---

## WISE710A1 Boot From SD/eMMC

The SW2 is a couple dip switch on the WISE710A1 for booting selection. 

### SW2: 1: ON, 2: OFF

The WISE710A1's SPL (Second Program Loader) will check SD card's boot loader first. If it exists, and then it will load boot loader on SD card, and then the boot loader will run kernel image and mount root file system on SD card. 

If there is no boot loader on SD card or SD card not inserted, SPL will load the boot loader on eMMC, and run kernel and mount root file system on eMMC. 

### SW2: 1: OFF, 2: ON

This will force WISE710A1 to boot from SD card only. And it needs the different SD card image from previous one. 
You have to write image by different script in BSP. The commands you need are listed as below. 

SD card device: /dev/sdc
```
$ cd build/sdcripts
$ ./mksd_recovery-linux_boot-from-sd.sh /dev/sdc ubuntu18044
```


---

## Docker

The package includes the package to create Docker container for building the images (ex: U-Boot, Kernel...). 

### How to Use

#### 1. Enter docker sub folder. 

#### 2. make bash


---
###### tags: `wise710a1`
