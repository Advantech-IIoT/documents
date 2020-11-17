# ISP

## Contents
- [install lpc21isp FLASH(適用於 LINUX)](#install-lpc21isp-FLASH-(適用於 LINUX))
---

## install lpc21isp FLASH(適用於 LINUX)

* sudo apt-get install -y lpc21isp  

* sudo lpc21isp --help

* sudo lpc21isp -hex xxx.hex /dev/ttyTHS2 115200 115200

## [github install](https://github.com/cmcqueen/lpc21isp.git)

* git clone https://github.com/cmcqueen/lpc21isp.git

* make 

* 燒入前，先把nano gpio 62 66 拉hing,燒入的時候再把66拉low，燒入完後62拉low。

* sudo ./lpc21isp -hex xxx.hex /dev/ttyTHS2 115200 115200

![](https://i.imgur.com/0QCGeN1.png)
