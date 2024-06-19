## systemd
systemd 单元文件位置，如果冲突，则/etc中的文件拥有最高的优先级。单元文件后缀跟进所配置的单元类型而变化。
文件由多区块（section）（例如[Unit]）组成。
单元可以是服务、套接字、目标、挂载点、设备、交换文件或分区、启动目标、受监视的文件系统路径、systemd 计时器、资源管理分片、外部创建的一组进程、进入另一个宇宙的虫洞。

### 软件包
`/usr/lib/systemd/system`


### 本地单元及自定义配置
`/etc/systemd/system`


### cpu turbo
 https://huataihuang.gitbooks.io/cloud-atlas/content/os/linux/kernel/cpu/intel_turbo_boost_and_pstate.html

 https://docs.aws.amazon.com/zh_cn/linux/al2/ug/processor_state_control.html