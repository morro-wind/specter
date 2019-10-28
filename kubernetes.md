## 配置要求：

* system version:
```
    Ubuntu 16.04+
    Debian 9+
    CentOS 7
    Redhat(RHEL) 7
    Fedora 25+
    Container Linux (tested with 1800.6.0)
```

* 2 GB or more of RAM
* 2 CPUs or more
* 所有集群间网络互通（私有或公有网络）
* 每个节点主机名、MAC address和product_uuid 要唯一
* Swap disabled.You MUST disable swap in order for the kubelet to work properly.

## 验证每个节点MAC address和product_uuid的唯一性

* network interface MAC address `ip link` or `ifconfig -a`
* check product_uuid `sudo cat /sys/class/dmi/id/product_uuid`

Kubernetes 使用这些值唯一地标识集群中的节点。

## 开放端口

Control-plane node(s)

Protocol| Direction| Port Range| Purpose| Used By
:-: | :-: | :-: | :-: | :-:
TCP| Inbound| 6443*| Kubernetes API server| All
TCP| Inbound| 2379-2380	| etcd server client API| kube-apiserver, etcd
TCP| Inbound| 10250| Kubelet API| Self, Control plane
TCP| Inbound| 10251| kube-scheduler| Self
TCP| Inbound| 10252| kube-controller-manager| Self

Worker node(s)

Protocol| Direction| Port Range| Purpose| Used By
:-: | :-: | :-: | :-: | :-:
TCP| Inbound| 10250| Kubelet API| Self, Control plane
TCP| Inbound| 30000-32767| NodePort Services**| ALL

`**` NodePort 服务默认端口范围
标有`*` 的端口号都是可修改的

## 安装容器

## 安装kubeadm, kubelet 和kubectl
在所有主机上安装：
* kubeadm: 引导集群命令
* kubelet: 执行启动如Pod 和容器之类的操作
* kubectl: 用于与集群通信命令行工具

确保版本一致

## 在控制平面节点上配置kubelet 使用cgroup驱动
使用Docker，kubeadm 自动检测kubelet 的cgroup驱动，运行时设置在`/var/lib/kubelet/kubeadm-flags.env` 文件中。  