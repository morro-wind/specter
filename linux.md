# 调整 Linux I/O 调度器优化系统性能 {#ibm-pagetitle-h1}

[https://www.ibm.com/developerworks/cn/linux/l-lo-io-scheduler-optimize-performance/index.html](https://www.ibm.com/developerworks/cn/linux/l-lo-io-scheduler-optimize-performance/index.html)

lpi exam 701 devops

[https://www.lpi.org/our-certifications/exam-701-objectives](https://www.lpi.org/our-certifications/exam-701-objectives)

## 标准输入口令
echo -n "passwd" | passwd --stdin

## awk ip
awk --re-interval '{match($0,/([0-9]{1,3}\.){3}[0-9]{1,3}/,a); print a[0]}'

/etc/exports

## sec
/etc/tcsd.preload
/usr/local/lib/lmnettesting.so
/usr/local/lib/crct13dif_intel.so
/etc/ld.so.preload
/etc/init.d/virtio_helper
/usr/local/bin//virtio_helper

ps aux | grep "scsi\|glue_filterlib32c"


## wget 递归下载

wget -c -r -np --reject=html -nH https://example

## yum repo group

You can either open a text editor and create the groups xml file manually or you can run the yum-groups-manager command from yum-utils.

Run the groups-create command like this:

```shell
yum-groups-manager -n "My Group" --id=mygroup --save=mygroups.xml --mandatory yum glibc rpm
```

To include this in a repository, just tell createrepo to use it when making or remaking your repository.

`createrepo -g /path/to/mygroups.xml /srv/my/repo`