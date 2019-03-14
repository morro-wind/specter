## 文件和进程数限制（ulimit）
### 文件数限制（ulimit -n）

要能够一次打开大量文件。许多Linux发行版限制允许单个用户打开的文件数`1024`（或者`256`在旧版本的OS X上）。您可以通过`ulimit -n`以运行HBase的用户身份登录时运行命令来检查服务器上的此限制。

建议将ulimit提升至至少10,000，但更可能是10,240，因为该值通常以1024的倍数表示.


### 顯示打開文件描述 數
cat /proc/sys/fs/file-max
正常用戶可以在單個登錄會話中打開的文件數

### 查看hard 和 soft 值
ulimit -Hn
ulimit -Sn

## 系統級別文件描述符FD 限制
在整個系統中同時打開的文件描述符的數量可通過/etc/sysctl.conf 文件進行更改

### 修改文件最大數
在內核變量文件`/proc/sys/fs/file-max` 中設置新值增加打開文件的最大數
`#sysctl -w fs.file-max=100000`

### 修改/etc/sysctl.conf 配置文件，永久生效
`#echo "fs.file-max=100000" >> /etc/sysctl.conf`

### 立即生效
`#sysctl -p`

### 驗證
`#sysctl fs.file-max`

## 用戶級別FD 限制 限制爲特定限制
### 进程数限制（ulimit -u）

在Linux和Unix中，使用该`ulimit -u`命令设置进程数。这不应与`nproc`命令混淆，该命令控制给定用户可用的CPU数量。在加载时，`ulimit -u`该值太低会导致OutOfMemoryError异常。

### `ulimit`Ubuntu上的设置

要在Ubuntu上配置ulimit设置，请编辑_/etc/security/limits.conf_，这是一个包含四列的空格分隔文件。有关此文件格式的详细信息，请参阅_limits.conf_的手册页。在以下示例中，第一行使用用户名hadoop为操作系统用户将打开文件数（nofile）的软限制和硬限制设置为32768。第二行将同一用户的进程数设置为32000。

```
hadoop  -       nofile  32768
hadoop  -       nproc   32000
```

仅在指向可插入验证模块（PAM）环境时才应用这些设置。要配置PAM以使用这些限制，请确保_/etc/pam.d/common-session_文件包含以下行：

```
session required  pam_limits.so
```

系統支持的協議類型記錄在/etc/protocols 文件中

系統支持的服務記錄在/etc/services 文件中

錯誤號的錯誤信息記錄在/usr/include/asm-generic/errno-base.h 錯誤定義文件中

