# Build U-boot and Kernel image for WISE710A1 and TPC71W

---


## Quick start

### Toolchain

Install `fsl-imx-x11` in `/opt` on your Ubuntu16.04 or Ubuntu 18.04 host PC or virtual machine first.

### Fetch code from github

```bash=
$ git clone https://github.com/Advantech-IIoT/Yocto2.1-ASG-i.mx6.git
```

### Check model information


#### Supported models

```bash=
$ make models
tpc71wn21pa wise710a1 tpc71wn10pa wise710a1_2G
```
#### More detial information for spectific model

ex: wise710a1
```bash=
$ make info modelname=wise710a1
```

### Build u-boot and kernel image


#### Build both kernel and u-boot images

```bash=
$ make modelname=wise710a1 build
```

```bash=
$ ls build/uboot_build build/kernel_build
```

For u-boot, we need `SPL, u-boot_crc.bin, u-boot_crc.bin.crc and u-boot.imx`. 

For Kernel, we need `dtb and zImage`. 

ex: use the find command
```bash=
$ find build/kernel_build -name "*.dtb" -or -name "zImage"
build/kernel_build/arch/arm/boot/zImage
build/kernel_build/arch/arm/boot/dts/imx6dl-wise710-a1.dtb
```

#### For more detial command

```bash=
$ make 

##########################################################
#                                                         #
# Compile i.MX6 Kernel & U-Boot Source Tool               #
#                                                         #
###########################################################

  Build Kernel and U-Boot:

    $ make modelname=tpc71wn21pa build

  Compile Kernel:

    $ make  modelname=tpc71wn21pa compile_kernel

  Compile Kernel and Build Modules for RootFS:

    $ make  modelname=tpc71wn21pa build_kernel

  Compile U-Boot:

    $ make  modelname=tpc71wn21pa compile_uboot

  Install Kernel Modules:

    $ make  modelname=tpc71wn21pa install_kernel_modules

  Supported Model Name:

    tpc71wn21pa, wise710a1, tpc71wn10pa, wise710a1_2G

  Check information:

    ex:
      $ make info modelname=tpc71wn21pa

```


---

###### tags: `tpc71wn21pa` `tpc71wn10pa` `wise710a1`
