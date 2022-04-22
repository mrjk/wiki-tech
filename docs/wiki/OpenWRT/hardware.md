# Hardware


## TP-Link Archer C7

Diffrences between A7 and C7:
https://www.tp-link.com/us/compare/?typeId=9&productIds=26649%2C31387

Model:
```
root@OpenWrt:~# dmesg | grep -i machin
[    0.000000] MIPS: machine is TP-Link Archer C7 v2

root@OpenWrt:~# cat /etc/os-release 
NAME="OpenWrt"
VERSION="21.02.2"
ID="openwrt"
ID_LIKE="lede openwrt"
PRETTY_NAME="OpenWrt 21.02.2"
VERSION_ID="21.02.2"
HOME_URL="https://openwrt.org/"
BUG_URL="https://bugs.openwrt.org/"
SUPPORT_URL="https://forum.openwrt.org/"
BUILD_ID="r16495-bf0c965af0"
OPENWRT_BOARD="ath79/generic"
OPENWRT_ARCH="mips_24kc"
OPENWRT_TAINTS=""
OPENWRT_DEVICE_MANUFACTURER="OpenWrt"
OPENWRT_DEVICE_MANUFACTURER_URL="https://openwrt.org/"
OPENWRT_DEVICE_PRODUCT="Generic"
OPENWRT_DEVICE_REVISION="v0"
OPENWRT_RELEASE="OpenWrt 21.02.2 r16495-bf0c965af0"
```

## Bad performances of C7 v2

Sources:

* https://forum.openwrt.org/t/please-consider-ath10k-non-ct-drivers-instead-of-ct/85051
* https://forum.openwrt.org/t/why-the-switch-to-unstable-ath10k-ct/27258/39
* https://forum.openwrt.org/t/why-the-switch-to-unstable-ath10k-ct/27258/8
* https://forum.openwrt.org/t/changing-to-non-ct-drivers-via-opkg/64070
* https://github.com/greearb/ath10k-ct/issues/139



Poor wifi performances since 2019:
```
# opkg list-installed  | grep ath10k
ath10k-board-qca988x - 20211216-1
ath10k-firmware-qca988x-ct - 2020-11-08-1
kmod-ath10k-ct - 5.4.179+2021-09-22-e6a7d5b5-1

# opkg remove ath10k-firmware-qca988x-ct  kmod-ath10k-ct
Removing package ath10k-firmware-qca988x-ct from root...
Removing package kmod-ath10k-ct from root...

# opkg list-installed  | grep ath10k
ath10k-board-qca988x - 20211216-1

# opkg install ath10k-firmware-qca988x kmod-ath10k
Installing ath10k-firmware-qca988x (20211216-1) to root...
Downloading https://downloads.openwrt.org/releases/21.02.2/packages/mips_24kc/base/ath10k-firmware-qca988x_20211216-1_mips_24kc.ipk
Installing kmod-ath10k (5.4.179+5.10.85-1-1) to root...
Downloading https://downloads.openwrt.org/releases/21.02.2/targets/ath79/generic/packages/kmod-ath10k_5.4.179%2b5.10.85-1-1_mips_24kc.ipk
Configuring kmod-ath10k.
Configuring ath10k-firmware-qca988x.

# reboot

```
Then:
```
# dmesg|grep firmware
[    0.348797] 0x000000020000-0x000000ff0000 : "firmware"
[    0.356593] 2 tplink-fw partitions found on MTD device firmware
[    0.362635] Creating 2 MTD partitions on "firmware":
[   15.532206] ath10k_pci 0000:00:00.0: firmware ver 10.2.4-1.0-00047 api 5 features no-p2p,raw-mode,mfp,allows-mesh-bcast crc32 35bd9258
[   35.741643] ath10k_pci 0000:00:00.0: pdev param 0 not supported by firmware

```





