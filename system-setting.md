- [Secure](#secure)
    + [Secure sshd](#secure-sshd)
    + [Firewall](#firewall)
    + [SELinux](#SElinux)
- [System Setting](#system-setting)
    + [Kernel Setting](#kernel-setting)
    + [tmpfs create temp file](#tmpfs-create-temp-file)
- [Software Setting](#software-setting)
    + [nginx](#nginx)
    + [tomcat](#tomcat)
    + [csvn](#csvn)
    + [file share](#file-share)
    + [jenkins](#jenkins)
    + [php7](#php7)
    + [FusionInventory-Agent](#FusionInventory-Agent)
    + [Svn Server LDAP](#Svn-Server-LDAP)
    + [Install xfce](#Install-xfce)


# Secure

## 标准化

系统安装原则：
1. 最小化系统安装，根据需要安装开发库
2. /var/log目录：挂载独立磁盘，防止log爆盘影响系统
3. /data目录：挂载独立磁盘，用于存放应用数据目录

系统安全设置原则：
1. 禁用root 账号远程登陆
2. 禁用密码登陆，开启密钥登陆，不同业务线不可使用同一密钥
3. 开启SELinux
4. 开启firewall，只放行服务端口
5. 创建普通用户，赋予sudo 权限
6. 创建应用运行用户，以非root 权限用户运行应用
7. 访问限制，只允许ssh 登陆IP，只允许跳板机主机登陆（考虑无法登陆跳板机时的应急方案），deny所有IP

## Secure sshd


# System Setting

系统设置

## Kernel Setting

TCP FastOpen

RFC743所述，在Linux Kernel 3.7以上版本中，支持服务器和客户端开启TCP FastOpen（TFO）。

## tmpfs create temp file
### systemd

`systemd` 将`.conf` 文件存放在`/etc/tmpfiles.d`,`/run/tmpfiles.d`,`/usr/lib/tmpfiles.d`目录中的集中机制创建临时目录，替代启动脚本中的`mkdir` 命令。

`# grep -r /var/run /usr/lib/tmpfiles.d/*`

```txt
/usr/lib/tmpfiles.d/initscripts.conf:d /var/run/netreport 0775 root root -
/usr/lib/tmpfiles.d/libselinux.conf:d /var/run/setrans 0755 root root
/usr/lib/tmpfiles.d/pam.conf:d /var/run/console 0755 root root -
/usr/lib/tmpfiles.d/pam.conf:d /var/run/faillock 0755 root root -
/usr/lib/tmpfiles.d/pam.conf:d /var/run/sepermit 0755 root root -
/usr/lib/tmpfiles.d/var.conf:L /var/run - - - - ../run
```

**字段含义**

整文档请参阅**`man tmpfiles.d`**

- 字段1: d 表示创建一个目录（如果目录不存在）
- 字段2: 目录的创建路径
- 字段3: 权限
- 字段4: 用户
- 字段5: 组

### 老PRE-SYSTEMD

看起来它们是由各个服务在启动时动态创建的：

```shell
$ sudo egrep -r 'mkdir.*/var/run' /etc

/etc/init.d/ssh:        mkdir /var/run/sshd
/etc/init.d/bind9:      mkdir -p /var/run/named
/etc/init.d/timidity:    mkdir -p /var/run/timidity
/etc/init.d/bzflag:                mkdir -p /var/run/bzflag
/etc/init.d/dns-clean:mkdir /var/run/pppconfig >/dev/null 2>&1 || true
/etc/init/winbind.conf: mkdir -p /var/run/samba/winbindd_privileged
/etc/init/dbus.conf:    mkdir -p /var/run/dbus
/etc/init/ssh.conf:    mkdir -p -m0755 /var/run/sshd
/etc/init/libvirt-bin.conf:     mkdir -p /var/run/libvirt
/etc/init/cups.conf:    mkdir -p /var/run/cups/certs
```

相信这是处理mysqld的那个：

```shell
[ -d /var/run/mysqld ] || install -m 755 -o mysql -g root -d /var/run/mysqld
/lib/init/apparmor-profile-load usr.sbin.mysqld
```
`man install`表示-d表单将“创建指定目录的所有组件”。

# Software Setting

服务软件配置，包括nginx、tomcat等，在非root 账号下运行

## nginx

[nginx](http://www.nginx.org) 配置开启io、缓存优化等

### configure

```text
# ./configure --prefix=/data/nginx \
--error-log-path=/data/logs/nginx/error.log \
--http-log-path=/data/logs/nginx/access.log \
--pid-path=/run/nginx.pid \
--lock-path=/data/nginx/nginx.lock \
--user=nginx \
--group=web \
--with-threads \
--with-file-aio \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_geoip_module \
--with-http_image_filter_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_auth_request_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_slice_module \
--with-http_stub_status_module \
--http-client-body-temp-path=/data/nginx/tmp/client_body_temp \
--http-proxy-temp-path=/data/nginx/tmp/proxy_temp \
--http-fastcgi-temp-path=/data/nginx/tmp/fastcgi_temp \
--http-uwsgi-temp-path=/data/nginx/tmp/uwsgi_temp \
--http-scgi-temp-path=/data/nginx/tmp/scgi_temp \
--with-mail \
--with-mail_ssl_module \
--with-stream \
--with-stream_ssl_module \
--with-stream_realip_module \
--with-stream_geoip_module \
--with-stream_ssl_preread_module \
--with-pcre \
--with-pcre-jit
```

### Configure nginx init scripts

参考[官方文档](https://www.nginx.com/resources/wiki/start/topics/examples/initscripts/?highlight=script%20nginx)

`# cat /lib/systemd/system/nginx.service`

```text
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

创建nginx用户、web组、tmp目录，启动nginx 服务

```bash
# groupadd web
# useradd -M -s /sbin/nologin -g web nginx
# mkdir /data/nginx/tmp
# chown nginx.web /data/nginx/tmp
# systemctl daemon-reload
# systemctl start nginx
```

### cache

使用tmpfs 内存文件系统配置cache位置

使用map 配置选择cache 位置

```
# Define caches and their locations
proxy_cache_path /mnt/ssd/cache keys_zone=ssd_cache:10m levels=1:2 inactive=600s
                 max_size=700m;
proxy_cache_path /mnt/disk/cache keys_zone=disk_cache:100m levels=1:2 inactive=24h
                 max_size=80G;

# Requests for .mp4 and .avi files go to disk_cache
# All other requests go to ssd_cache
map $request_uri $cache {
    ~.mp4(?.*)?$  disk_cache;
    ~.avi(?.*)?$  disk_cache;

    default ssd_cache;
}

server {
    # select the cache based on the URI
    proxy_cache $cache;

    # ...
}
```

### access log

configure access log format

```

```


## tomcat

[tomcat](http://tomcat.apache.org)

## csvn

[csvn](https://www.collab.net/downloads/subversion)

## file share

[cells](https://github.com/pydio/cells)

cells server 要求启用https，客户端可使用sync 同步

server 配置如下：

```
+---+---------------------+--------+--------------------------+
| # |       BIND(S)       |  TLS   |       EXTERNAL URL       |
+---+---------------------+--------+--------------------------+
| 0 | http://0.0.0.0:8080 | No Tls | https://file.liepass.com |
+---+---------------------+--------+--------------------------+
```

nginx proxy 配置如下：

```
server {
  if ($host = file.liepass.com) {
      return 301 https://$host$request_uri;
  }

  listen 80;
  server_name file.liepass.com;
  return 404;
}

server {
  listen 443 ssl;
  server_name file.liepass.com;

  error_log /data/logs/nginx/error.log;
  access_log /data/logs/nginx/access.log main;
  ssl_certificate      cert.pem;
  ssl_certificate_key  cert.key;
  ssl_protocols        TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers          HIGH:!aNULL:!MD5;

  client_max_body_size 200M;

  proxy_send_timeout 600;
  proxy_read_timeout 600;
  proxy_request_buffering off;
  keepalive_timeout 600s;

  location / {
      proxy_buffering off;
      proxy_pass http://10.114.32.97:8080$request_uri;
      proxy_set_header X-Real-IP $remote_addr;
  }

  location /ws {
      proxy_buffering off;
      proxy_pass http://10.114.32.97:8080;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_read_timeout 86400;
  }
}

server {
  listen 54545 ssl http2;
  listen [::]:54545 ssl http2;
  ssl_certificate      cert.pem;
  ssl_certificate_key  cert.key;
  ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers         HIGH:!aNULL:!MD5;
  keepalive_timeout 600s;

  location / {
    grpc_pass grpc://10.114.32.97:54545;
  }

  error_log /data/logs/nginx/proxy-grpc-error.log;
  access_log /data/logs/nginx/proxy-grpc-access.log main;
}
```


## jenkins

插件列表:

```
Role-based Authorization Strategy
Extended Choice Parameter
```

权限配置

正则表达式已.*结尾，表示匹配以任意字符结尾的项目或文件夹


## php7

### Prerequisites:  

```shell
# yum install systemd-devel libxml2-devel libsqlite3x-devel \
# bzip2-devel libcurl-devel libicu cyrus-sasl-ldap oniguruma-devel libsodium-devel 
# cp -frp /usr/lib64/libldap* /usr/lib/
```

install libzip-1.7,cmake 3.19

```shell
# wget -c https://github.com/Kitware/CMake/releases/download/v3.19.8/cmake-3.19.8.tar.gz
# tar zxf cmake-3.19.8.tar.gz
# cd cmake-3.19.8 && ./configure && make && make install
# wget -c https://libzip.org/download/libzip-1.7.3.tar.gz
# tar zxf libzip-1.7.3.tar.gz
# cd libzip && mkdir build && cd build && cmake .. && make && make install
#
```

### build php7

```shell
# ./configure --prefix=/data/php7 --with-curl --enable-gd --with-zip --enable-fpm --with-fpm-systemd \
# --enable-mbstring --enable-mysqlnd --with-pdo-mysql --with-mysqli \
# --with-config-file-path=/data/php7/etc --with-openssl --enable-soap \
# --with-zlib --enable-intl --with-xmlrpc --enable-exif --with-bz2 \
# --with-sodium --with-ldap --with-ldap-sasl PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig
# make && make install
```

### Configure php

```shell
# cp php.ini-production /data/php7/etc/
# cp sapi/fpm/php-fpm.conf /data/php7/etc/
# cp sapi/fpm/www.conf /data/php7/etc/php-fpm.d/
# cp sapi/fpm/php-fpm.service /lib/systemd/system/
```

### mysql sql

```sql
> CREATE USER 'pydio'@'localhost' IDENTIFIED BY '%[V5Zi#NEd@yq';
> CREATE DATABASE cells;
> GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
> FLUSH PRIVILEGES;
```

## FusionInventory-Agent

### Dependencies

```shell
# yum install perl-ExtUtils-MakeMaker perl-File-Which perl-Crypt-SSLeay perl-Test-MockObject perl-Parse-EDID perl-Test-CPAN-Meta perl-HTTP-ProxyAutoConfig perl-Sys-Syslog -y
# wget http://cpanmin.us/ -O cpanm; chmod +x cpanm
# cpanm .
```

## Svn Server LDAP

### 编译安装svn 1.10.6版本

依赖要求
subversion-1.10.6.tar.gz
sqlite-amalgamation-3081101.zip

#### 安装依赖

```shell
# yum install gcc gcc-c++ apr-devel apr-util-devel zlib-devel lz4-devel utf8proc-devel cyrus-sasl-plain cyrus-sasl cyrus-sasl-lib cyrus-sasl
```

#### 配置编译安装

```shell
# mv sqlite-amalgamation-3081101 subversion-1.10.6/sqlite-amalgamation
# ./configure --with-sasl
# make && make install
```

#### 加载动态库

```shell
echo "/usr/local/lib" >> /etc/ld.so.conf.d/svn.conf
```

#### 验证版本信息

```shell
# /usr/local/bin/svnserve --version
```

#### system service

```shell
# cat /usr/lib/tmpfiles.d/svn.conf 
d /run/svn   755 svn svn
# cat /lib/systemd/system/svn.service 
[Unit]
Description=Subversion Server
After=network.target

[Service]
User=svn
Group=svn
Type=forking
PIDFile=/run/svn/svn.pid
ExecStart=/usr/local/bin/svnserve -d --listen-port=3690 --root=/svn/svnrepo/svnRepository --log-file /var/log/svnlog/log.txt --pid-file=/run/svn/svn.pid
ExecStop=/bin/kill -s QUIT $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target

```

### 集成AD 验证

`svn sever` 使用`sasl` 集成`ldap` 验证.

**`saslauthd` 验证方式**

```shell
# saslauthd -v
saslauthd 2.1.26
authentication mechanisms: getpwent kerberos5 pam rimap shadow ldap httpform
```

**修改验证方式**

```shell
# cat /etc/sysconfig/saslauthd 
SOCKETDIR=/run/saslauthd
MECH=ldap
FLAGS=
```

**sasl配置域信息**

```shell
#  /etc/saslauthd.conf 
ldap_servers: ldap://ldap.example.com
ldap_default_domain: example.com
ldap_search_base: DC=example,DC=com
ldap_bind_dn: CN=srv,OU=services,DC=example,DC=com
ldap_password: ybz8m%example
ldap_deref: never
ldap_restart: yes
ldap_scope: sub
ldap_use_sasl: no
ldap_start_tls: no
ldap_version: 3
ldap_auth_method: bind
ldap_mech: DIGEST-MD5
ldap_filter:sAMAccountName=%u
ldap_password_attr:userPassword
ldap_timeout: 10
ldap_cache_ttl: 30
ldap_cache_mem: 32786
```

**重启sasl 服务**

重启服务，AD 连接测试

```shell
# systemctl restart saslauthd.service
# testsaslauthd -u test -p 'pwd'
0: OK"Success."
```

**svn 集成验证**

```shell
# cat /etc/sasl2/svn.conf 
pwcheck_method:saslauthd
mech_list: plain login
# systemctl restart saslauthd.service
```

**svn 版本库认证配置**

```shell
# cat svnserve.conf 
[general]
anon-access = none
auth-access = write
authz-db = authz
[sasl]
use-sasl = true
min-encryption = 0
max-encryption = 0
```

## Install xfce

更新系统repo，更新系统
`# yum update -y`

**安装epel**
`#yum install epel-release -y`

**安装xfce**
`#yum groupinstall xfce -y`

**安装桌面显示管理器**
`#yum install lightdm -y`

**切换默认桌面启动**
`#systemctl set-default graphical.target`

**切换终端启动**
`#systemctl set-default multi-user.target`

## 用户组添加sudo指定命令
`#sed -i '/Cmnd_Alias DRIVERS/'a\ "\ \n## EXAMPLE \nCmnd_Alias EXAMPLE=/usr/bin/yum" /etc/sudoers`
`#sed -i '/^root/'a\ "\ \n## Allow example group run \n%example ALL=(ALL)           EXAMPLE" /etc/sudoers`

[[ ! `grep "^X11UseLocalhost no" /etc/ssh/sshd_config` ]] && sed -i '/^X11Forwarding/'a\ "X11UseLocalhost no" /etc/ssh/sshd_config





Manage Roles $\rightarrow$ ->


```mermaid
graph LR
A[方形] ->B(圆角)
    B -> C{条件a}
    C ->|a=1| D[结果1]
    C ->|a=2| E[结果2]
    F[横向流程图]
```

```flow
st=>start: Start:>https://www.google.com[blank]
e=>end:>https://www.google.com
op1=>operation: My Operation|current
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?:>https://www.google.com
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```



# Linux
## 弱口令文件
/usr/share/dict/words

## Filesystem
udev
tmpfs
devtmpfs

## Disk UUID
`blkid /dev/sdb`

yum-config-manager --enable docker-ce-nightly
yum-config-manager --disable docker-ce-nightly
yum list docker-ce --showduplicates | sort -r
## 文件和进程数限制（ulimit）
```
https://www.cyberciti.biz/faq/linux-increase-the-maximum-number-of-open-files/

linux-security
https://www.cyberciti.biz/tips/linux-security.html
```

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

# UBUNTU
## /boot disk space less

```
#dpkg --get-selections | linux
#dpkg --purge linux-header-$oldversion
```
