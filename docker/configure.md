修改默认docker0 地址及默认仓库地址：

```
vim /etc/docker/daemon.json or /etc/sysconfig/docker

{

"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"] ,

"bip":"172.31.0.1/16"

}
```



