
[TOC]

## 需求描述
某些场景下需要修改mac 地址，本文指导其修改方式

## 前置条件
1. 主机需要双网卡
2. vmware 主机不可修改外部设备mac 地址
3. 以修改系统网络配置实现

## 配置修改

将网络配置修改为如下内容，替换{MACADDR}为实际所需内容，eth0 替换为实际需要修改的网络接口名称
```shell
# cat /etc/sysconfig/network-scripts/ifcfg-eth0
```

```shell
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=none
MACADDR={MACADDR}
```

## 重启网络配置以生效
```shell
# systemctl restart network
or
# ifdown eth0;ifup eth0
```

## 验证
eth0 替换为实际的网络接口名称
```shell
# ip addr show eth0
```




