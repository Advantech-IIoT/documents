# ISP

## Contents
- [install lpc21isp FLASH](#install-lpc21isp-FLASH)
---

## install lpc21isp FLASH

## Host OS: Ubuntu 16.04 or 18.04 Arm linux

* sudo apt-get install -y lpc21isp  

* sudo lpc21isp --help

* sudo lpc21isp -hex xxx.hex /dev/ttyTHS2 115200 115200

## [Github install](https://github.com/cmcqueen/lpc21isp.git)

* git clone https://github.com/cmcqueen/lpc21isp.git

* make 

* Before burning, the nano gpio 62 66 should be hinged, nano 66 should be low when burning, nano 62 should be low after burning.

* sudo ./lpc21isp -hex xxx.hex /dev/ttyTHS2 115200 115200

![](https://i.imgur.com/0QCGeN1.png)
