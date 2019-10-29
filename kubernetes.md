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
* 每个节点主机名、MAC address和product\_uuid 要唯一
* Swap disabled.You MUST disable swap in order for the kubelet to work properly.

## 验证每个节点MAC address和product\_uuid的唯一性

* network interface MAC address `ip link` or `ifconfig -a`
* check product\_uuid `sudo cat /sys/class/dmi/id/product_uuid`

Kubernetes 使用这些值唯一地标识集群中的节点。

## 开放端口

Control-plane node\(s\)

| Protocol | Direction | Port Range | Purpose | Used By |
| :---: | :---: | :---: | :---: | :---: |
| TCP | Inbound | 6443\* | Kubernetes API server | All |
| TCP | Inbound | 2379-2380 | etcd server client API | kube-apiserver, etcd |
| TCP | Inbound | 10250 | Kubelet API | Self, Control plane |
| TCP | Inbound | 10251 | kube-scheduler | Self |
| TCP | Inbound | 10252 | kube-controller-manager | Self |

Worker node\(s\)

| Protocol | Direction | Port Range | Purpose | Used By |
| :---: | :---: | :---: | :---: | :---: |
| TCP | Inbound | 10250 | Kubelet API | Self, Control plane |
| TCP | Inbound | 30000-32767 | NodePort Services\*\* | ALL |

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

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable --now kubelet
```
**注意**

* 运行`setenforce 0 `设置SELinux为permissive 模式。这是允许容器访问主机系统文件必须的，例如pod 网络。
* 在RHEL/CentOS 7上，某些用户绕过iptables而导致流量路由错误。你应确保在`sysctl`配置中`net.bridge.bridge-nf-call-iptables`值为1
```
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```
* 在此之前确认已加载`br_netfilter` 模块。运行`lsmod | grep br_netfilter`验证。显示加载执行`modprobe br_netfilter`.

现在`kubelet` 每个几秒重启一次，它在等待`kubeadm`告诉它做什么


## 在control-plane 节点上，配置kubelet 使用cgroup 驱动
当使用Docker时，kubeadm 在设置`/var/lib/kubelet/kubeadm-flags.env`文件中自动检测kubelet cgroup驱动程序

## 安装并设置kubectl
开始前：
使用的kubectl 版本必须是在集群内的同一个大版本

1. 下载最新版本
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```

2. 使用kubectl 二进制文件
```
chmod +x ./kubectl
```

3. 移动二进制文件到PATH
```
sudo mv ./kubectl /usr/local/bin/kubectl
```

4. 测试确保安装的为最新版本
```
kubectl version
```

包管理器安装
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubectl
```