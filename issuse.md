# issuse

yum install epel-release
yum install perl-Module-Install
You are visiting the local directory
  '/root/src/fusioninventory-agent-2.6/.'
  without lock, take care that concurrent processes do not do likewise.
Running make for /root/src/fusioninventory-agent-2.6/.

  CPAN.pm: Building /root/src/fusioninventory-agent-2.6/.

Can't locate inc/Module/Install.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 8.
BEGIN failed--compilation aborted at Makefile.PL line 8.
Warning: No success on command[/usr/bin/perl Makefile.PL]
Directory '/root/src/fusioninventory-agent-2.6/.' not below /root/.cpan/build, will not store persistent state
  /root/src/fusioninventory-agent-2.6/.
  /usr/bin/perl Makefile.PL -- NOT OK

## gitlab

### This user cannot be unlocked manually from GitLab

```
# gitlab-rails console
  Loading production environment (Rails 4.2.7.1)

  irb(main):001:0> user = User.find_by_email("email@something.com")

  => .... output snipped ...

irb(main):002:0> user.state = "active"
=> "active"
irb(main):003:0> user.save
=> true
irb(main):004:0> exit
```



perl Makefile.PL 
Can't locate inc/Module/Install.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 8.
BEGIN failed--compilation aborted at Makefile.PL line 8.



## sssd

issuse
https://www.kernel.org/doc/Documentation/security/keys.txt
https://www.mail-archive.com/sssd-users@lists.fedorahosted.org/msg07597.html
```text
[[sssd[krb5_child[42521]]]] [unpack_buffer] (0x1000): total buffer size: [156]
[[sssd[krb5_child[42521]]]] [tgt_req_child] (0x1000): Attempting to get a TGT
[sssd[krb5_child[42509]]]: Disk quota exceeded
[sssd[krb5_child[42509]]]: Disk quota exceeded
```


`cat /proc/key-users`
```
10846:   206 206/206 205/1000 4713/20000
```

`cat /proc/keys`

fix

```text
up */proc/sys/kernel/keys/maxkeys *(from default of 200 to 1000) 
```


sec

ps aux | grep scs
/etc/tcsd.preload 
/usr/local/lib/lmnettesting.so
ld.so.preload、crct10dif_intel.so、virtio_helper

### sssd 密码认证失败
secure log :pam_sss(sshd:auth): received for user $USERNAME: 4 (System error)

ssh clinet -vvv
Permission denied, please try again.

/tmp 目录权限错误

## x11 forwarding request failed on channel 0
ERROR: Unable to open X display

edit sshd_config modify X11MaxDisplays value
X11MaxDisplays
       Specifies the maximum number of displays available for sshd(8)'s X11 forwarding.  This prevents sshd from exhausting local ports.  The
       default is 1000.



## Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed. Unable to create GSSAPI-encrypted LDAP connection.
https://docs.microsoft.com/en-us/answers/questions/570467/linux-server-join-to-ad-using-sssd-the-linux-serve.html
https://lists.fedoraproject.org/archives/list/freeipa-users@lists.fedorahosted.org/thread/REMZAIDC2DXFCFT5ZAOO4ZZAWQXGPMAA/

`# sssctl domain-status testlab.local`
`ipa-getkeytab`


https://sssd.io/release-notes/sssd-2.4.0.html



### ERR_SSL_PROTOCOL_ERROR
域名访问ERR_SSL_PROTOCOL_ERROR，域名未备案，tls 加密算法无法通过拦截
`ssl_ciphers         ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:CDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:AES128-GCM-SHA256:ES128-SHA:AES256-SHA256:AES256-SHA;`


## git pull 冲突 error: Your local changes to the following files would be overwritten by merge

git stash 

git stash list

git pull

git pop

git diff

git commit 

git push





nfsiostat
iftop
ifstat
iperf






## debug x11forward
- remote x11 gui 无操作时异常退出
    - bsub -Is -q ali_test -m ali-cm001 gvim,等待故障复现
    - 观察lsf 日志-无相关内容
    - 观察网络连接状态，发现反向sshd 连接
    - sshd TCPKeepAlive 配置会检查网络和连接主机状态，并在故障时中断连接
    - sshd ClientAliveInterval 会发送消息请求响应，ClientAliveCountMax 配置发送消息次数，达到次数后无数据会断开连接，0禁用连接终止
    

## git config
git config http.postBuffer 1048576000
git config https.postBuffer 1048576000


## rancher server ip 变更
https://blog.csdn.net/RancherLabs/article/details/125302014


## git常见错误及解决办法

错误1
`git push -u origin master`

```
# fatal: unable to access 'https://github.com/xxx/xxx.git/' OpenSSL SSL read: Connection was reset,err 10054
```

解决办法：
SSL验证失败导致，可以直接禁止SSL验证

`git config --global http.sslVerify "false"`

官网关于http.sslVerify的说明如下：
```
Whether to verify the SSL certificate when fetching or pushing over HTTPS. Defaults to true. Can be overridden by the GIT_SSL_NO_VERIFY environment variable.
```

错误2
```
error: RPC failed curl 18 transfer closed with outstanding read data remaining
send-pack: unexpected disconnect while reading sideband packet
fatal: the remote end hung up unexpectedly
Everything up-to-date
```

解决办法：

缓存过小导致，可以尝试增大缓存

`git config --gobal http.postBuffer 524288000`
`# 单位byte，524288000=500M`

官网关于 http.postBuffer的说明:
```
Maximum size in bytes of the buffer used by smart HTTP transports when POSTing data to the remote system. For requests larger than this buffer size, HTTP/1.1 and Transfer-Encoding: chunked is used to avoid creating a massive pack file locally. Default is 1 MiB, which is sufficient for most requests.

Note that raising this limit is only effective for disabling chunked transfer encoding and therefore should be used only where the remote server or a proxy only supports HTTP/1.0 or is noncompliant with the HTTP standard. Raising this is not, in general, an effective solution for most push problems, but can increase memory consumption significantly since the entire buffer is allocated even for small pushes.
```

网络波动导致，可以尝试取消相关的网络限制

```
git config --gobal http.lowSpeedLimit 0
git config --gobal http.lowSpeedTime 999999
```

官网关于http.lowSpeedLimit和http.lowSpeedTime的说明:

```
If the HTTP transfer speed is less than http.lowSpeedLimit for longer than http.lowSpeedTime seconds, the transfer is aborted. Can be overridden by the GIT_HTTP_LOW_SPEED_LIMIT and GIT_HTTP_LOW_SPEED_TIME environment variables.
```

若传输文件实在太大，可以尝试增大压缩率(压缩率大小根据实际情况设置)

`git config --gobal core.compression 3`

官网关于core.compression的说明:

```
An integer -1…9, indicating a default compression level. -1 is the zlib default. 0 means no compression, and 1…9 are various speed/size tradeoffs, 9 being slowest. If set, this provides a default to other compression variables, such as core.looseCompression and pack.compression.
```

## gitlab log
https://www.cnblogs.com/bigyoung/p/14115944.html

背景：
公司抓信息安全，使用gitlab进行代码管理，要求所有用户的远程操作（推送、同步）都记录下来。

通过查看Gitlab官方文档，整理信息如下：

gitlab 后台的各种日志保存位置 /var/log/gitlab/

production.log
注意：本日志只记录通过http操作的日志

存放目录：/var/log/gitlab/gitlab-rails/

production_json.log里面是Json请求串。

{
    "method": "GET",
    "path": "/test_user/test_project.git/info/refs",
    "format": "*/*",
    "controller": "Projects::GitHttpController",
    "action": "info_refs",
    "status": 200,
    "duration": 268.22,
    "view": 0.48,
    "db": 14.41,
    "time": "2019-06-27T10:59:56.324Z",
    "params": [
        {
            "key": "service",
            "value": "git-receive-pack"
        },
        {
            "key": "namespace_id",
            "value": "test_user"
        },
        {
            "key": "project_id",
            "value": "test_project.git"
        }
    ],
    "remote_ip": "192.168.XX.XX",
    "user_id": 3,
    "username": "test_user",
    "ua": "git/2.21.0.windows.1",
    "queue_duration": null,
    "correlation_id": "b02c02f9-0167-49bf-965f-e4cc86d6751f"
}
日志中有价值的信息：

同步动作：service：git-receive-pack
推送操作：service：git-upload-pack
项目名： project_id：test_project.git
IP地址：remote_ip：192.168.XX.XX
用户名： username：test_user
时间：time：2019-06-27T10:59:56.324Z（UTC格式，加上8个小时等于北京时间）
状态：status：200 （200表示操作成功，其他表示失败）
动作信息：action：info_refs （每次同步、推送操作出现的标志，需要通过这个字段来来筛选日志是否是更新或者推送操作）
对存在Json嵌套的数据操作，建议看看这篇文章，能够提高工作效率。
Go 如何优雅的获取嵌套Json数据内容

gitlab-shell.log
**注意：此日志只记录Gitclone协议的操作

日志目录：/var/log/gitlab/gitlab-shell
以下日志就不是Json格式了，需要自己对字符串进行操作处理。

time="2019-07-02T11:17:48+08:00" level=info msg="executing git command" command="gitaly-receive-pack unix:/var/opt/gitlab/gitaly/gitaly.socket {\"repository\":{\"storage_name\":\"default\",\"relative_path\":\"test_user/test_project.git\",\"git_object_directory\":\"\",\"git_alternate_object_directories\":[],\"gl_repository\":\"project-5\",\"gl_project_path\":\"test_user/test_project\"},\"gl_repository\":\"project-5\",\"gl_project_path\":\"test_user/test_project\",\"gl_id\":\"key-3\",\"gl_username\":\"test_user\",\"git_config_options\":[],\"git_protocol\":null}" pid=23657 user="user with id key-3"
日志中有价值的信息：

同步动作：command：gitaly-receive-pack
推送操作：command：gitaly-upload-pack
项目名： gl_project_path：test_user/test_project
IP地址：remote_ip：192.168.XX.XX
用户名： gl_username：test_user
时间：time：2019-07-02T11:17:48+08:00（UTC格式，加上8个小时等于北京时间）
状态：status：200 （200表示操作成功，其他表示失败）
动作信息：action：info_refs （每次同步、推送操作出现的标志，需要通过这个字段来来筛选日志是否是更新或者推送操作）
参考文档：

Gitlab官方日志解释文档 https://docs.gitlab.com/ee/administration/logs/


### Too many objects for client

```journal -u dbus``` 

I have almost 300 physical host,and each physical host running about 60 docker containers . I think maybe it is too much ,and almost docker containers are running mysql test ,I don't kown if it is resulting in too much connections ,but I don't think it is this reason;

I think it relates to this issue:
https://github.com/systemd/systemd/issues/1961
and
https://bugs.freedesktop.org/show_bug.cgi?id=95263

As we can see, it has bellow phenomenon:
1. when I login these problem docker container delay 25 s
2. login in container run ```journal -u dbus``` you will see

```
Apr 02 07:51:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 10
Apr 02 07:57:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 270
Apr 02 08:34:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 10
Apr 02 08:35:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 10
Apr 02 10:33:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 10
Apr 02 11:44:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 273
Apr 02 12:04:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 273
Apr 02 12:09:02 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 273
Apr 02 16:16:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 273
Apr 02 22:08:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 273
Apr 03 00:51:01 docker011160.xxx dbus-daemon[80]: Failed to close file descriptor: Could not close fd 10
....
...

Apr 19 01:45:57 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:46:01 docker011160.xxx dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:46:01 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:46:26 docker011160.xxx dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:46:26 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:46:32 docker011160.xxx dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:46:32 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:46:57 docker011160.xxx dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:46:57 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:47:02 docker011160.xxx dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:47:02 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:47:27 docker011160.xxx dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:47:27 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:47:29 docker011160.xxx dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:47:29 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
Apr 19 01:47:54 docker011160.xxx dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:47:54 docker011160.xxx dbus-daemon[129907]: dbus[129907]: [system] Failed to activate service 'org.freedesktop.login1': timed out
Apr 19 01:48:00 docker011160.xxx dbus[129907]: [system] Activating via systemd: service name='org.freedesktop.login1' unit='dbus-org.freedesktop.login1.service'
```

top will see systemd-logind dbus-daemon too high:

```
   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 91255 root      20   0   50344  18000   1404 R  76.0  0.0   0:25.84 systemd-logind
    79 dbus      20   0   34348   9024   1216 S  12.7  0.0   2761:36 dbus-daemon
  6941 mysql     20   0 1989064 280536   7288 S   1.5  0.1 865:02.70 mysqld

```

systemd-logind.service restart almost every minutes

```
Apr 29 06:29:57 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:30:57 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:31:58 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:32:59 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:34:00 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:35:02 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:36:04 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:37:04 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:38:05 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:39:07 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:40:08 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:41:09 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:42:10 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:43:10 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:44:12 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:45:14 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:46:14 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
Apr 29 06:47:15 docker01116.xxx systemd[1]: systemd-logind.service has no holdoff time, scheduling restart.
```

There are too many abandon scopes

```
#systemctl | grep abandon | wc -l
25207

#systemctl | grep abandon  | grep mysql | wc -l
8799

#systemctl | grep abandon  | grep agent | wc -l
9224
```

man systemd.scope  we all know, Scope units are not configured via unit configuration files, but are only created programmatically using the bus interfaces of systemd

busctl can't see immornal problem

```
#busctl
NAME                              PID PROCESS         USER             CONNECTION    UNIT                      SESSION    DESCRIPTION
:1.0                                1 systemd         root             :1.0          -                         -          -
:1.2011552                      99310 polkitd         polkitd          :1.2011552    polkit.service            -          -
:1.2028946                      86791 systemd-logind  root             :1.2028946    systemd-logind.service    -          -
:1.2028954                      86976 su              root             :1.2028954    session-2950.scope        2950       -
:1.2028955                      87001 su              root             :1.2028955    session-2950.scope        2950       -
:1.2028956                      87011 su              root             :1.2028956    session-2950.scope        2950       -
:1.2028957                      87254 busctl          root             :1.2028957    sshd.service              -          -
net.reactivated.Fprint              - -               -                (activatable) -                         -
org.freedesktop.DBus                - -               -                -             -                         -          -
org.freedesktop.PolicyKit1      99310 polkitd         polkitd          :1.2011552    polkit.service            -          -
org.freedesktop.hostname1           - -               -                (activatable) -                         -
org.freedesktop.locale1             - -               -                (activatable) -                         -
org.freedesktop.login1          86791 systemd-logind  root             :1.2028946    systemd-logind.service    -          -
org.freedesktop.machine1            - -               -                (activatable) -                         -
org.freedesktop.systemd1            1 systemd         root             :1.0          -                         -          -
org.freedesktop.timedate1           - -               -                (activatable) -                         -

```


Look at logs ,it looks like first ```dbus-daemon[80]: Failed to close file descriptor: Could not close fd xxx
``` and then ```dbus alway active service 'org.freedesktop.login1' : timed out```


My try:
1. Just only restart dbus or systemd-login ,this problem will have.
2. If excute command 
```systemctl | grep "abandoned" | grep -e "-[[:digit:]]" | sed "s/\.scope.*/.scope/" | xargs systemctl stop```  this problem will be solved short-lived

This can not fundamentally solve this problem

SO, I try to backport this patch to dbus:
1. Only backport this patch: https://bugs.freedesktop.org/page.cgi?id=splinter.html&bug=95263&attachment=124841  ----->   problem still exist,abandon scopes still increasing
2. Only backport this patch: https://bugs.freedesktop.org/page.cgi?id=splinter.html&bug=95263&attachment=124842
-----> problem still exsit,abandon scopes still increasing

we can see dbus changelog(https://cgit.freedesktop.org/dbus/dbus/tree/NEWS?h=master):
