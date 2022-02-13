
https://www.cnblogs.com/kaishirenshi/p/10396446.html
docker container update --restart=always 容器名字

## docker selinux label
[官方文档说明](https://docs.docker.com/storage/bind-mounts/)
```
The z option indicates that the bind mount content is shared among multiple containers.
The Z option indicates that the bind mount content is private and unshared.
```

修改默认docker0 地址及默认仓库地址：

https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file


```
vim /etc/docker/daemon.json or /etc/sysconfig/docker

{

"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"] ,

"bip":"172.31.0.1/16"

}
```

`amazon-linux-extras install selinux-ng`

`yum install container-selinux`

```
/etc/docker/daemon.json
{
  "data-root": "/data/docker",
  "selinux-enabled": true,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3",
    "compress": "true"
  }
}
```



这些都是失败。如果您的进程尝试写入 /var/data，您将获得权限被拒绝。

正确的标签应该是这样的

system_u:object_r:container_file_t:s0

和

system_u:object_r:container_file_t:s0:c1,c2

container_var_lib_t 是 /var/lib/docker 中内容的默认标签。我们想阻止这种类型的密闭容器。

chcon -R -t container_file_t logs/

#!!!! The file '/opt/jumpserver/config/redis/redis.conf' is mislabeled on your system.  
#!!!! Fix with $ restorecon -R -v /opt/jumpserver/config/redis/redis.conf
allow container_t container_var_lib_t:file read;
