
[TOC]

## Toollist

- lspci
- lsmod
- lshw
- nvidia-smi

## 获取网卡硬件名称

获取网卡硬件设备名称，为下载网卡驱动做准备

```shell
# lspci -k| grep -A3 -i eth
# lspci -vnn| grep -A3 -i eth
 ``` 

```shell 
19:00.1 Ethernet controller: Broadcom Inc. and subsidiaries NetXtreme BCM5720 2-port Gigabit Ethernet PCIe
01:00.0 Ethernet controller: Intel Corporation 82599ES 10-Gigabit SFI/SFP+ Network Connection (rev 01)
     Subsystem: Intel Corporation Ethernet Server Adapter X520-2
     Kernel driver in use: ixgbe
     Kernel modules: ixgbe  
```

## 查看网卡驱动信息

此处以eth0 为例，执行时请替换eth0 为实际设备名称
```shell
# ethtool -i eth0
```

```shell
driver: ixgbe
version: 5.1.0-k-rh7.7
firmware-version: 0x800008fd, 18.0.16
expansion-rom-version: 
bus-info: 0000:86:00.0
supports-statistics: yes
supports-test: yes
supports-eeprom-access: yes
supports-register-dump: yes
supports-priv-flags: yes
```

## 查看内核加载网络驱动信息

```shell
# modinfo ixgbe
```

```shell
filename:       /lib/modules/3.10.0-1127.19.1.el7.x86_64/kernel/drivers/net/ethernet/intel/ixgbe/ixgbe.ko.xz
version:        5.1.0-k-rh7.7
license:        GPL v2
description:    Intel(R) 10 Gigabit PCI Express Network Driver
author:         Intel Corporation, <linux.nics@intel.com>
retpoline:      Y
rhelversion:    7.8
......
```

## 下载网卡驱动

根据获取的网卡硬件设备名称，登录对应厂商官网搜索对应设备驱动并下载

## 安装驱动

解压驱动文件，根据README 文件指导安装驱动

## 检查驱动版本并加载

```shell
# modinfo ixgbe 
# modprobe ixgbe
```

在加载新模块前，从内核中删除旧版本驱动

```shell
# rmmod i40e; modprobe i40e
```

注：某些发行版在安装新驱动后，需要更新initrd/initramfs 文件，以防止系统加载旧版本驱动

```shell
   For Red Hat distributions:
        # dracut --force

   For Ubuntu:
        # update-initramfs -u
```