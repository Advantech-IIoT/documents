# Ubuntu 16.04 for TPC71WN21PA
---

## Quick Start

### Fetch build package source

```bash=
$ git clone git@github.com:advantechralph/tpc71w-u16.git
# or
$ git clone https://github.com/advantechralph/tpc71w-u16.git
$ cd tpc71w-u16
```

### Make image installer 

For `makefile` usage
```bash=
$ make 
```

Make specific model

```bash=
$ make info modelname=tpc71wn21pa_soreel
```

### Add a new model

All supported models are in `models` folder, you can add a new model in this folder by adding `${modelname}.mk`

ex: 
modelname=tpc71wn21pa_soreel
add tpc71wn21pa_soreel.mk in models 

```=
ifeq ($(modelname),tpc71wn21pa_soreel)
bspname:=TPC71W-N21PA-r1-Ubuntu1604-SOREEL-$(shell date +"%Y%m%d")
rootfs=$(CURRDIR)/rootfs/n21pa
spl=$(CURRDIR)/spl/n21pa/SPL
dtb=$(CURRDIR)/dtb/n21pa/imx6q-tpc71w-n21pa.dtb
u-boot=$(CURRDIR)/u-boot/n21pa
logo=$(CURRDIR)/logo/soreel/adv_logo_1024x600_32bpp.bmp
endif
```

---
###### tags: `tpc71wn21pa` `ubuntu` `docs`